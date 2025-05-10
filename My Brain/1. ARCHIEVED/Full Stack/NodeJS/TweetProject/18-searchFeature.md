---
date: 2025-05-10T22:58:00
---
Related : [[NodeJS]]
Tag: #nodejs #prj #searchfeature
___

# I - chức năng Search

- mô tả dùng chức năng search
  - người dùng nhập thông tin vào ô tìm kiếm vd: "nguoi dep trai nhat" và nhấn nút send
  - thì đoạn chuỗi trên sẽ được `encodeURIComponent("nguoi dep trai nhat")` và gắn vào url trông thế này: http://localhost:8000/search/?q=nguoi%20dep%20trai%20nhat
  - vậy nên khi server nhận đc query trên thì phải biến đổi ngược lại để tìm kiếm
    `decodeURIComponent("nguoi%20dep%20trai%20nhat")`
  - sau đó ta có thể tiến hành dùng chuỗi đã decode để tìm kiếm trong database
  - search sẽ tìm đc: tweet, users, tweet chứa ảnh, video
  - search nâng cao: theo ngày, theo từ ngữ
- **search là 1 tình năng rất là khó, thường họ sẽ dùng thư viện ngoài, nhưng ta chỉ có mongo thôi, nên chức năng search của mình sẽ không quá xịn xò**

## 1 - thực hành

ta sẽ tìm kiếm dựa vào `content` của 1 bài `tweet` nên ta sẽ tạo 1 `index` cho `content` trong `tweetModel`

- tạo index cho dự án của mình để đồng bộ

  - vào file `database.services.ts` thêm hàm `indexTweets`

    ```ts
    async indexTweets() {
      const exists = await this.tweets.indexExists(['content_text'])
      if (!exists) {
        this.tweets.createIndex({ content: 'text' }, { default_language: 'none' })
      }
    }
    ```

    **`default_language: 'none'` điều này giúp mongoDB tránh bị hiện tượng `stop words`**

    - stop words là những từ thông dụng, không có ý nghĩa, nên khi search nó sẽ bỏ qua những từ này, vd: `a, the, and, or, ...`, nếu bạn tìm kiếm `a` thì sẽ không tìm ra kết quả nào cả

  - vào index.ts để gọi hàm `indexTweets` này

    ```ts
    ...
    databaseService.indexTweets()
    ...
    ```

- mongodb hỗ trợ ta 2 cách search là `mongoDB Atlas search(dùng cho những bạn chỉ hosting trên db mongo atlas thôi)` và `text search on Self-Managed Deployments(dùng ở đâu cũng được hết)`,
- ta có thể tham khảo ở [doc](https://www.mongodb.com/docs/manual/text-search/)
- và [đây nữa](https://www.mongodb.com/docs/manual/reference/operator/query/text/#mongodb-query-op.-text)
- thử update 1 tweet
  ![[image-375.png]]
- giờ ta sẽ search thử trên `mongo compass` xem sao
  ![[image-376.png]]
- search có dấu và nhiều chữ, ta sẽ thấy mongo search theo từng chữ
  ![[image-377.png]]
- nên nếu ta $caseSensitive: true thì vẩn sẽ tìm ra như thường
  ![[image-378.png]]
- nhưng nếu ta chỉ tìm chữ `Điên` mà phân biệt hoa thường thì sẽ không tìm ra
  ![[image-379.png]]

## 2 - tạo route search

- vào folder `routes` tạo fike `search.routes.ts`

  ```ts
  import { Router } from 'express'
  import { searchController } from '~/controllers/search.controllers'
  import { accessTokenValidator, verifiedUserValidator } from '~/middlewares/users.middlewares'
  import { wrapAsync } from '~/utils/handlers'

  const searchRouter = Router()

  searchRouter.get('/', accessTokenValidator, verifiedUserValidator, wrapAsync(searchController))

  export default searchRouter
  ```

- vào `index.ts` để `app tổng` xài `route` này

  ```ts
  ...
  app.use('/search', searchRouter) //route handler
  ...
  ```

- định nghĩa `query` cho `request` của `search`, ta vào `models > requests` tạo file `Search.request.ts`

  ```ts
  import { Pagination } from '~/models/requests/Tweet.requests'

  //kết quả tìm kiếm có thể lên đến hàng trăm nên mình cần phần trang
  //tức là cần limit và page
  export interface SearchQuery extends Pagination {
    content: string
  }
  ```

- vào folder `controllers` tạo `search.controllers.ts`

  ```ts
  import { Request, Response } from 'express'
  import { SearchQuery } from '~/models/requests/Search.request'
  import { ParamsDictionary } from 'express-serve-static-core'

  export const searchController = async (req: Request<ParamsDictionary, any, any, SearchQuery>, res: Response) => {
    res.json({ message: 'search success' })
  }
  ```

- tạo `request` trên `postman` và test thử
  ![[image-380.png]]

## 3 - hàm search trong database

- trong `service` tạo `search.services.ts`

```ts
import { SearchQuery } from '~/models/requests/Search.request'
import databaseService from './database.services'

class SearchServices {
  //ta k co query: SearchQuery vì {content:string, limit:string, page:string}
  async search(query: { content: string; limit: number; page: number }) {
    const { content, limit, page } = query
    const result = await databaseService.tweets
      .find({ $text: { $search: content } })
      .skip(limit * (page - 1))
      .limit(limit)
      .toArray()
    return result
  }
}

const searchServices = new SearchServices()
export default searchServices
```

- xài hàm ở `searchController`

```ts
export const searchController = async (req: Request<ParamsDictionary, any, any, SearchQuery>, res: Response) => {
  const limit = Number(req.query.limit)
  const page = Number(req.query.page)
  const result = await searchServices.search({ content: req.query.content, limit, page })
  res.json({ message: 'search success', result })
}
```

- test lại
  ![[image-381.png]]
- thử tìm 1 cái gì đó và phân trang xem
  ![[image-382.png]]

## chuyển từ .find qua .aggregate để lookup lấy đủ thông tin phân trang, tolal

- lên mongoDB compass tạo aggregation mới cho collection tweets
- vì rất giống `aggretion của get new feed` nên ta mở `aggregate` đó lên và chỉnh sửa lại
  stage đầu tiên thành
  ![[image-383.png|600]]
- sau đó save as đặt tên là `search content`
- ta sẽ thu được đoạn code sau

  ```js
  ;[
    {
      $match: {
        $text: {
          $search: 'cho'
        }
      }
    },
    {
      $lookup: {
        from: 'users',
        localField: 'user_id',
        foreignField: '_id',
        as: 'user'
      }
    },
    {
      $match: {
        $or: [
          {
            audience: 0
          },
          {
            $and: [
              {
                audience: 1
              },
              {
                'user.tweeter_circle': {
                  $in: [new ObjectId('65351b0f4db32da0e9d5e74f')]
                }
              }
            ]
          }
        ]
      }
    },
    {
      $skip: 1
    },
    {
      $limit: 2
    },
    {
      $unwind: {
        path: '$user'
      }
    },
    {
      $lookup: {
        from: 'hashtags',
        localField: 'hashtags',
        foreignField: '_id',
        as: 'hashtags'
      }
    },
    {
      $lookup: {
        from: 'users',
        localField: 'mentions',
        foreignField: '_id',
        as: 'mentions'
      }
    },
    {
      $addFields: {
        mentions: {
          $map: {
            input: '$mentions',
            as: 'mentions',
            in: {
              _id: '$$mentions._id',
              name: '$$mentions.name',
              username: '$$mentions.username',
              email: '$$mentions.email'
            }
          }
        }
      }
    },
    {
      $lookup: {
        from: 'bookmarks',
        localField: '_id',
        foreignField: 'tweet_id',
        as: 'bookmarks'
      }
    },
    {
      $addFields: {
        bookmarks: {
          $size: '$bookmarks'
        }
      }
    },
    {
      $lookup: {
        from: 'likes',
        localField: '_id',
        foreignField: 'tweet_id',
        as: 'likes'
      }
    },
    {
      $addFields: {
        likes: {
          $size: '$likes'
        }
      }
    },
    {
      $lookup: {
        from: 'tweets',
        localField: '_id',
        foreignField: 'parent_id',
        as: 'tweet_children'
      }
    },
    {
      $addFields: {
        retweet_count: {
          $size: {
            $filter: {
              input: '$tweet_children',
              as: 'item',
              cond: {
                $eq: ['$$item.type', 1]
              }
            }
          }
        },
        comment_count: {
          $size: {
            $filter: {
              input: '$tweet_children',
              as: 'item',
              cond: {
                $eq: ['$$item.type', 2]
              }
            }
          }
        },
        quote_count: {
          $size: {
            $filter: {
              input: '$tweet_children',
              as: 'item',
              cond: {
                $eq: ['$$item.type', 3]
              }
            }
          }
        },
        view: {
          $add: ['$user_views', '$guest_views']
        }
      }
    },
    {
      $project: {
        tweet_children: 0,
        user: {
          password: 0,
          email_verify_token: 0,
          forgot_password_token: 0,
          twitter_circle: 0,
          date_of_birth: 0
        }
      }
    }
  ]
  ```

- giờ ta sẽ dùng đoạn code trên để sử dụng `aggregation` trong `search.services.ts > searchServices > search` thay cho `.find`

  ```ts
    //thêm user_id để biết clien đc phép xem tweet nào
    //cập nhật params
    async search(query: { content: string; limit: number; page: number; user_id: string }) {
      const { content, limit, page, user_id } = query //thêm user_id
      const result = await databaseService.tweets
        .aggregate([
          //phần code lấy đc
          {
            $match: {
              $text: {
                $search: content //cập nhật
              }
            }
          }
          ...
          {
            $match: {
              $or: [
                {
                  audience: 0
                },
                {
                  $and: [
                    {
                      audience: 1
                    },
                    {
                      'user.tweeter_circle': {
                        $in: [new ObjectId(user_id)] //cập nhật
                      }
                    }
                  ]
                }
              ]
            }
          },
          {
            $skip: (page - 1) * limit //cập nhật
          },
          {
            $limit: limit // cập nhật
          },
          ...
          {
            $addFields: {
              retweet_count: {
                $size: {
                  $filter: {
                    input: '$tweet_children',
                    as: 'item',
                    cond: {
                      $eq: ['$$item.type', TweetType.Retweet] //cập nhật
                    }
                  }
                }
              },
              comment_count: {
                $size: {
                  $filter: {
                    input: '$tweet_children',
                    as: 'item',
                    cond: {
                      $eq: ['$$item.type', TweetType.Comment] //cập nhật
                    }
                  }
                }
              },
              quote_count: {
                $size: {
                  $filter: {
                    input: '$tweet_children',
                    as: 'item',
                    cond: {
                      $eq: ['$$item.type', TweetType.QuoteTweet] //cập nhật
                    }
                  }
                }
              },
            ...
        ])
        .toArray()
      return result
    }
  ```

- cập nhật lại `searchController` để thêm đầu vào `user_id` để lọc theo `audience`

  ```ts
  export const searchController = async (req: Request<ParamsDictionary, any, any, SearchQuery>, res: Response) => {
    const limit = Number(req.query.limit)
    const page = Number(req.query.page)

    const result = await await searchServices.search({
      content: req.query.content,
      limit,
      page,
      user_id: req.decoded_authorization?.user_id as string //nếu có thì truyền vô
    })
    res.json({ message: 'search success', result })
  }
  ```

- kết quả thu được đã có đủ thông tin của tweet
  ![[image-384.png]]

- **ta tiến hành làm tăng view và phân trang cho những `tweet` vừa search**

  - vào `search.services.ts`

    ```ts
    async search(query: { content: string; limit: number; page: number; user_id: string }) {
        const { content, limit, page, user_id } = query //thêm user_id

        //đổi tên
        const tweets = await databaseService.tweets
          .aggregate([
            ])
          .toArray()
        // lấy danh sách ids của các tweet đã search
        const tweet_ids = tweets.map((tweet) => tweet._id as ObjectId)
        // tăng views cho các tweet đã search trên mongo
        const inc = user_id ? { user_views: 1 } : { guest_views: 1 }
        const date = new Date()
        await databaseService.tweets.updateMany(
          {
            _id: {
              $in: tweet_ids
            }
          },
          {
            $inc: inc,
            $set: {
              updated_at: date
            }
          }
        )
        // tăng views cho các tweet đã search trên server
        tweets.forEach((tweet) => {
          tweet.updated_at = date
          user_id ? tweet.user_views++ : tweet.guest_views++
        })

        //tính tổng số tweet đã tìm đc
        const total = await databaseService.tweets
          .aggregate([
            //giống phần code aggregate ở trên, nhưng sau stage $math cuối cùng ta count chứ k thêm thông tin
            {
              $match: {
                $text: {
                  $search: content
                }
              }
            },
            {
              $lookup: {
                from: 'users',
                localField: 'user_id',
                foreignField: '_id',
                as: 'user'
              }
            },
            {
              $match: {
                $or: [
                  {
                    audience: 0
                  },
                  {
                    $and: [
                      {
                        audience: 1
                      },
                      {
                        'user.tweeter_circle': {
                          $in: [new ObjectId(user_id)]
                        }
                      }
                    ]
                  }
                ]
              }
            },
            {
              $count: 'total' //count luôn
            }
          ])
          .toArray()

        //đếm số tweets đã tìm đc
        return {
          tweets,
          total: total[0]?.total || 0 //nếu có thì lấy total[0].total, k thì lấy 0
        }
      }
    ```

  - vào `search.controllers.ts`

    ```ts
    export const searchController = async (req: Request<ParamsDictionary, any, any, SearchQuery>, res: Response) => {
      ...
      res.json({
        message: 'search success',
        result: {
          tweets: result.tweets,
          limit,
          page,
          total_page: Math.ceil(result.total / limit)
        }
      })
    }
    ```

  - test lại xem kết quả đã có thông tin phân trang chưa
    ![[image-393.png]]

## 4 - tìm kiếm theo `hashtag` thì sẽ như thế nào ?

- vậy còn search `hashtag` thì sao, điều đó vô cùng đơn giản, vì bản thân content đã chứa hashtag rồi

- nếu 1 ai đó click vào `hashtag` thì `frontend` sẽ call api để `tìm kiếm tweet` theo `hashtag đã click` (ta có thể tự tạo route cho chức năng này)

- tạo index cho `name` trong collections `hashtags`
  ![[image-385.png]]

- tạo aggregation cho collection `tweets` trên `mongoDB compass`
  - tìm hashtag có name tương ứng
    ![[image-389.png]]
  - liên kết hashtags với tweets thông qua giá trị `hashtags` của `tweet`
    ![[image-390.png]]
  - tách từ mảng object result ra thành từng object riêng biệt
    ![[image-391.png]]
  - đảo ngược gốc từ collections `hashtags`sang `tweets`: giống thay left join thành right join vậy đó
    ![[image-392.png]]

## 5 - encodeURIComponent trên postman

- khi ta đang query có thể sẽ phải gặp query có dấu `space` như thế này
  ![[image-394.png]]
- chúng làm cho uri của mình có dấu `space` và khi gửi lên server thì server sẽ không có khả năng bị lỗi
- chúng ta nên encode uri trước khi gửi lên server
- ta có thể chỉnh tự động trên postman bằng cách vào task `pre-request script`
  và thêm đoạn code sau

```js
const content = pm.request.url.query.get('content')
pm.request.query.remove('content')
pm.request.query.remove(`content = ${encodeURIComponent(content)}`)
```

![[image-395.png]]

- nodejs khi nhận được sẽ tự decode trở lại

## 6 - lọc tweet có chứa ảnh, video

- nâng cấp chức năng search, ta sẽ tìm kiếm tweet có chứa ảnh, video
- ta sẽ dựa trên các ví dụ tìm kiếm cơ bản như sau
  - ta có một url tìm kiếm tweet
    `/search?content=anhhotgirlxinhdep&limit=5&page=1`
  - nếu muốn tìm ảnh thì nó sẽ thành
    `/search?content=anhhotgirlxinhdep&limit=5&page=1&filter=image`
  - nếu muốn tìm video thì nó sẽ thành
    `/search?content=anhhotgirlxinhdep&limit=5&page=1&filter=video`
- vậy thì tóm lại muốn tìm gì thì ngta sẽ truyền query `filter` tương ứng lên là xong

- vậy ta sẽ tiền hành tạo enum `MediaTypeQuery` để lưu chuỗi thay vì `MediaType` có sẵn chỉ lưu số

  - vào file `enums.ts` tạo

    ```ts
    export enum MediaType {
      Image,
      Video,
      HLS //cập nhật thêm
    }

    export enum MediaTypeQuery {
      Image = 'image',
      Video = 'video'
    }
    ```

  - chỉnh lại `SearchQuery` trong `Search.request.ts`

    ```ts
    export interface SearchQuery extends Pagination {
      content: string
      media_type: MediaTypeQuery
    }
    ```

- fix lại `searchController`

  ```ts
  export const searchController = async (req: Request<ParamsDictionary, any, any, SearchQuery>, res: Response) => {
    ...
    const result = await await searchServices.search({
      content: req.query.content,
      limit,
      page,
      user_id: req.decoded_authorization?.user_id as string,
      media_type: req.query.media_type //cập nhật ở đây
    })
    ...
  }

  ```

- fix lại `searchServices.search`

  ```ts
  //cập nhật params
    async search(query: { content: string; limit: number; page: number; user_id: string; media_type: MediaTypeQuery }) {
      const { content, limit, page, user_id, media_type } = query //thêm user_id
      //tạo thêm filter mặc định
      const $match: any = {
        $text: {
          $search: content //cập nhật
        }
      }
      //nếu có truyền lên media_type thì thêm vào $match
      if (media_type) {
        if (media_type === MediaTypeQuery.Image) {
          $match['medias.type'] = MediaType.Image
        } else if (media_type === MediaTypeQuery.Video) {
          $match['medias.type'] = {
            $in: [MediaType.Video, MediaType.HLS]
          }
        }
      }


      const tweets = await databaseService.tweets
        .aggregate([
          //phần code lấy đc
          {
            $match //cập nhật
          },
          ...
        ])
        .toArray()
      ...

      //tính tổng số tweet đã tìm đc
      const total = await databaseService.tweets
        .aggregate([
          {
            $match //cập nhật
          },
          ...
    }
  ```

- test tìm kiếm
  ![[image-396.png]]
- thử đổi type 1 thằng bất kỳ về 1(video)
  ![[image-397.png]]
- xong tìm thử
  ![[image-398.png]]

## 7 - tìm tweet của người mình follow hay 1 người bất kỳ

- rất đơn giản, chỉ cần truyền thêm 1 query là `people_follow` là xong

  - vào `enums.ts` thêm định nghĩa `people_follow` luôn

    ```ts
    export enum PeopleFollow {
      Anyone = '0',
      Following = '1'
    }
    ```

  - vào `Search.request.ts` thêm

    ```ts
    import { Query } from 'express-serve-static-core'
    //extends thêm Query

    export interface SearchQuery extends Pagination, Query {
      content: string
      media_type?: MediaTypeQuery // cập nhật thành optional
      people_follow?: PeopleFollow // vì người dùng chỉ truyền lên string là '0': Anyone  và  '1': Following
    }
    ```

- vào `searchController` thêm

  ```ts
  export const searchController = async (req: Request<ParamsDictionary, any, any, SearchQuery>, res: Response) => {
    ...
    const result = await await searchServices.search({
      content: req.query.content,
      limit,
      page,
      user_id: req.decoded_authorization?.user_id as string,
      media_type: req.query.media_type,
      people_follow: req.query.people_follow  //cập nhật k cần as PeopleFollow vì đc định nghĩa bằng SearchQuery
    })
    ...
  }
  ```

- vào `searchServices.search` và cập nhật thêm

  ```ts
  //cập nhật params
    async search(query: {
      content: string
      limit: number
      page: number
      user_id: string
      media_type?: MediaTypeQuery // cập nhật thành optional
      people_follow?: PeopleFollow // cập nhật
    }) {
      const { content, limit, page, user_id, media_type, people_follow } = query //cập nhật

      const $match: any = {
        $text: {
          $search: content
        }
      }

      if (media_type) {
        if (media_type === MediaTypeQuery.Image) {
          $match['medias.type'] = MediaType.Image
        } else if (media_type === MediaTypeQuery.Video) {
          $match['medias.type'] = {
            $in: [MediaType.Video, MediaType.HLS]
          }
        }
      }

      //thêm vào xử lý: nếu có truyền lên people_follow thì thêm vào $match
      if (people_follow && people_follow === PeopleFollow.Following) {
        const user_id_obj = new ObjectId(user_id)
        //tìm id cũa những người mà mình đang follow
        const followed_user_ids = await databaseService.followers
          .find(
            {
              user_id: user_id_obj
            },
            {
              projection: {
                _id: 0,
                followed_user_id: 1
              }
            }
          )
          .toArray()
        const ids = followed_user_ids.map((item) => item.followed_user_id)
        //thêm cả mình vào danh sách đó luôn
        ids.push(user_id_obj)
        //thêm vào $match
        $match['user_id'] = {
          $in: ids
        }
      }


      ...
      //đếm số tweets đã tìm đc
      return {
        tweets,
        total: total[0]?.total || 0 //nếu có thì lấy total[0].total, k thì lấy 0
      }
    }
  ```

- test thử
  - dùng login `lehodiep.2045@gmail.com` có id là `65351b0f4db32da0e9d5e74f`
  - tìm bài viết có chữ `điệp` và chỉ tìm người mình follow thôi
    ![[image-399.png]]
    bài viết này do user `65381c61b270f4a7fc2fc3bd` viết, và mình có follow thật
  - giờ ta unfollow user `65381c61b270f4a7fc2fc3bd` xem thử tìm lại còn không ?
    ![[image-400.png]]
  - tìm lại bài viết ở chế độ chỉ những người mình đã follow thì k thấy
    ![[image-401.png]]
  - tìm ở chế độ tất cả thì thấy
    ![[image-402.png]]
    vậy là thành công

## 8 - searchValidator

- giờ mình sẽ tạo validator cho search
- cập nhật lại `search route`

  ```ts
  searchRouter.get(
    '/',
    accessTokenValidator,
    verifiedUserValidator,
    searchValidator,
    paginationValidator,
    wrapAsync(searchController)
  ) //cập nhật
  ```

- chúng ta phải validate limit, page, content, media_type, people_follow
  - nhưng limit, page đã đc check bằng middleware `paginationValidator` rồi
  - nên ta chỉ cần check `content, media_type, people_follow` thôi
- trong folder `middlewares` tạo file `search.middlewares.ts`

  ```ts
  import { checkSchema } from 'express-validator'
  import { MediaTypeQuery, PeopleFollow } from '~/constants/enums'
  import { TWEETS_MESSAGES } from '~/constants/messages'
  import { validate } from '~/utils/validation'

  export const searchValidator = validate(
    checkSchema(
      {
        content: {
          isString: {
            errorMessage: TWEETS_MESSAGES.CONTENT_MUST_BE_STRING
          }
        },
        media_type: {
          optional: true,
          isIn: {
            options: [Object.values(MediaTypeQuery)], // đc mảng [['image','video']], // lưu ý phải có bọc array ở ngoài nữa
            errorMessage: `Media type must be in ${Object.values(MediaTypeQuery).join(', ')}`
          }
        },
        people_follow: {
          optional: true,
          isIn: {
            options: [Object.values(PeopleFollow)], // lưu ý phải có bọc array ở ngoài nữa
            errorMessage: TWEETS_MESSAGES.PEOPLE_FOLLOW_MUST_BE_0_OR_1
          }
        }
      },
      ['query']
    )
  )
  ```

