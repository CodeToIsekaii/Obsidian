```typescript
"use client"

import type React from "react"

interface TermsAndConditionsModalProps {
  onClose: () => void
}

export const TermsAndConditionsModal: React.FC<TermsAndConditionsModalProps> = ({ onClose }) => {
  return (
    <div className="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-[60] p-4">
      <div className="bg-white rounded-lg max-w-4xl w-full max-h-[80vh] overflow-y-auto">
        <div className="flex items-center justify-between p-6 border-b">
          <h2 className="text-xl font-semibold text-gray-900">Điều khoản và Điều kiện</h2>
          <button onClick={onClose} className="text-gray-600 hover:text-gray-800 text-2xl">
            ×
          </button>
        </div>

        <div className="p-6 space-y-4 text-sm text-gray-700">
          <h3 className="text-lg font-semibold text-gray-900">1. Điều khoản chung</h3>
          <p>
            Bằng việc sử dụng dịch vụ xét nghiệm ADN của GenUnity, quý khách đồng ý tuân thủ các điều khoản và điều kiện
            được quy định dưới đây.
          </p>

          <h3 className="text-lg font-semibold text-gray-900">2. Quy trình xét nghiệm</h3>
          <ul className="list-disc list-inside space-y-1">
            <li>Quý khách cần cung cấp đầy đủ thông tin cá nhân chính xác</li>
            <li>Mẫu xét nghiệm phải được lấy theo đúng quy trình của GenUnity</li>
            <li>Thời gian xét nghiệm từ 7-14 ngày làm việc tùy theo loại dịch vụ</li>
            <li>Kết quả xét nghiệm sẽ được thông báo qua email hoặc điện thoại</li>
          </ul>

          <h3 className="text-lg font-semibold text-gray-900">3. Bảo mật thông tin</h3>
          <p>
            GenUnity cam kết bảo mật tuyệt đối thông tin cá nhân và kết quả xét nghiệm của quý khách. Thông tin chỉ được
            sử dụng cho mục đích xét nghiệm và không được chia sẻ với bên thứ ba mà không có sự đồng ý của quý khách.
          </p>

          <h3 className="text-lg font-semibold text-gray-900">4. Chính sách thanh toán</h3>
          <ul className="list-disc list-inside space-y-1">
            <li>Quý khách cần thanh toán đầy đủ chi phí trước khi tiến hành xét nghiệm</li>
            <li>Các phương thức thanh toán được chấp nhận: VNPay, MoMo, chuyển khoản ngân hàng</li>
            <li>Không hoàn lại chi phí sau khi đã tiến hành lấy mẫu</li>
          </ul>

          <h3 className="text-lg font-semibold text-gray-900">5. Trách nhiệm của khách hàng</h3>
          <ul className="list-disc list-inside space-y-1">
            <li>Cung cấp thông tin chính xác và đầy đủ</li>
            <li>Tuân thủ các hướng dẫn của nhân viên kỹ thuật</li>
            <li>Thông báo kịp thời nếu có thay đổi thông tin liên lạc</li>
            <li>Chịu trách nhiệm về tính chính xác của mẫu xét nghiệm</li>
          </ul>

          <h3 className="text-lg font-semibold text-gray-900">6. Giới hạn trách nhiệm</h3>
          <p>
            GenUnity không chịu trách nhiệm về các quyết định được đưa ra dựa trên kết quả xét nghiệm. Kết quả xét
            nghiệm chỉ mang tính chất tham khảo và cần được tư vấn bởi các chuyên gia y tế.
          </p>

          <h3 className="text-lg font-semibold text-gray-900">7. Liên hệ hỗ trợ</h3>
          <p>
            Mọi thắc mắc về dịch vụ, vui lòng liên hệ:
            <br />
            Hotline: 1900-1234
            <br />
            Email: support@genunity.com
            <br />
            Địa chỉ: 123 DNA Street, Genome City, GC 12345
          </p>
        </div>

        <div className="flex justify-end p-6 border-t">
          <button
            onClick={onClose}
            className="bg-teal-600 hover:bg-teal-700 text-white py-2 px-6 rounded-lg font-medium transition-colors"
          >
            Đã hiểu
          </button>
        </div>
      </div>
    </div>
  )
}

```

```js
"use client"

import type React from "react"
import { useState, useEffect } from "react"
import { useNavigate } from "react-router-dom"
import { useAuth } from "../../hooks/useAuth"
import { TermsAndConditionsModal } from "./TermsAndConditionsModal"
import type { Service, ServiceRegistrationData, SampleProvider, CustomerInfo } from "../../utils/types"

interface ServiceRegistrationModalProps {
  service: Service
  onClose: () => void
}

export const ServiceRegistrationModal: React.FC<ServiceRegistrationModalProps> = ({ service, onClose }) => {
  const navigate = useNavigate()
  const { user } = useAuth()

  // Form state
  const [appointmentDate, setAppointmentDate] = useState("")
  const [collectionMethod, setCollectionMethod] = useState<"home" | "facility">(
    service.serviceType === "Administrative" ? "facility" : "home",
  )
  const [customerInfo, setCustomerInfo] = useState<CustomerInfo>({
    fullName: "",
    dateOfBirth: "",
    gender: "Nam",
    phoneNumber: "",
    idPhoto: null,
  })
  const [sampleProviders, setSampleProviders] = useState<SampleProvider[]>([])
  const [acceptTerms, setAcceptTerms] = useState(false)
  const [showTermsModal, setShowTermsModal] = useState(false)
  const [loading, setLoading] = useState(false)

  // Initialize form data
  useEffect(() => {
    // Auto-populate customer info from user profile (fetched from database)
    if (user?.profile) {
      setCustomerInfo({
        fullName: user.profile.fullName || "Nguyễn Văn A",
        dateOfBirth: user.profile.dateOfBirth || "01/01/1999",
        gender: user.profile.gender || "Nam",
        phoneNumber: user.profile.phoneNumber || "0123456789",
        idPhoto: user.profile.idPhoto || null,
      })
    } else {
      // Default values if no user profile (demo data)
      setCustomerInfo({
        fullName: "Nguyễn Văn A",
        dateOfBirth: "01/01/1999",
        gender: "Nam",
        phoneNumber: "0123456789",
        idPhoto: null,
      })
    }

    // Initialize sample providers based on service sample count
    const providers: SampleProvider[] = []
    for (let i = 0; i < service.sampleCount; i++) {
      providers.push({
        fullName: "",
        dateOfBirth: "",
        gender: "Nam",
        relationship: i === 0 ? "Cha" : i === 1 ? "Con" : "Mẹ",
        sampleType: "CMND/CCCD/Passport",
        idPhoto: null,
      })
    }
    setSampleProviders(providers)
  }, [user, service.sampleCount])

  const handleSampleProviderChange = (
    index: number,
    field: keyof SampleProvider,
    value: string | File | null,
  ): void => {
    const updatedProviders = [...sampleProviders]
    updatedProviders[index] = { ...updatedProviders[index], [field]: value }
    setSampleProviders(updatedProviders)
  }

  const handleFileUpload = (index: number, file: File | null, type: "customer" | "provider"): void => {
    if (type === "customer") {
      setCustomerInfo((prev) => ({ ...prev, idPhoto: file }))
    } else {
      handleSampleProviderChange(index, "idPhoto", file)
    }
  }

  const validateForm = (): boolean => {
    // Basic validation
    if (!customerInfo.fullName || !customerInfo.phoneNumber) {
      alert("Vui lòng điền đầy đủ thông tin khách hàng")
      return false
    }

    if (service.serviceType === "Administrative" && collectionMethod === "facility" && !appointmentDate) {
      alert("Vui lòng chọn ngày hẹn cho dịch vụ hành chính")
      return false
    }

    for (let i = 0; i < sampleProviders.length; i++) {
      const provider = sampleProviders[i]
      if (!provider.fullName || !provider.dateOfBirth) {
        alert(`Vui lòng điền đầy đủ thông tin người xét nghiệm ${i + 1}`)
        return false
      }
    }

    if (!acceptTerms) {
      alert("Vui lòng đồng ý với các điều khoản và điều kiện")
      return false
    }

    return true
  }

  const handleSubmit = async (): Promise<void> => {
    if (!validateForm()) return

    setLoading(true)

    try {
      // Prepare registration data
      const registrationData: ServiceRegistrationData = {
        serviceId: service.serviceId,
        customerInfo,
        sampleProviders,
        collectionMethod,
        appointmentDate: collectionMethod === "facility" ? appointmentDate : undefined,
        appointmentTime: collectionMethod === "facility" ? "09:00" : undefined,
        totalAmount: service.price,
        notes: `Dịch vụ: ${service.serviceName} - ${service.sampleCount} mẫu`,
      }

      // Navigate to payment page with registration data
      navigate("/payment", {
        state: {
          registrationData,
          totalAmount: service.price,
          service,
        },
      })
    } catch (error) {
      console.error("Error preparing registration:", error)
      alert("Có lỗi xảy ra. Vui lòng thử lại.")
    } finally {
      setLoading(false)
    }
  }

  const getTodayDate = (): string => {
    const today = new Date()
    return today.toISOString().split("T")[0]
  }

  return (
    <div className="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center z-50 p-4">
      <div className="bg-white rounded-lg max-w-2xl w-full max-h-[90vh] overflow-y-auto">
        {/* Header */}
        <div className="flex items-center justify-between p-6 border-b">
          <div>
            <button onClick={onClose} className="text-gray-600 hover:text-gray-800 mr-4">
              ← Quay lại
            </button>
            <h2 className="text-xl font-semibold text-gray-900 inline">
              Đăng ký lịch xét nghiệm -{" "}
              {service.serviceType === "Administrative" ? "Hành chính pháp lý" : "Dân sự tư nhân"}
            </h2>
          </div>
        </div>

        <div className="p-6 space-y-6">
          {/* Appointment Date (for Administrative services at facility) */}
          {service.serviceType === "Administrative" && collectionMethod === "facility" && (
            <div>
              <label className="block text-sm font-medium text-gray-700 mb-2">
                Ngày đăng ký <span className="text-red-500">*</span>
              </label>
              <input
                type="date"
                value={appointmentDate}
                onChange={(e) => setAppointmentDate(e.target.value)}
                min={getTodayDate()}
                className="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-teal-500 focus:border-transparent"
                required
              />
            </div>
          )}

          {/* Collection Method */}
          <div>
            <label className="block text-sm font-medium text-gray-700 mb-2">
              Hình thức lấy mẫu <span className="text-red-500">*</span>
            </label>
            <div className="flex space-x-4">
              {service.serviceType === "Civil" && (
                <label className="flex items-center">
                  <input
                    type="radio"
                    name="collectionMethod"
                    value="home"
                    checked={collectionMethod === "home"}
                    onChange={(e) => setCollectionMethod(e.target.value as "home" | "facility")}
                    className="mr-2"
                  />
                  Tại nhà
                </label>
              )}
              <label className="flex items-center">
                <input
                  type="radio"
                  name="collectionMethod"
                  value="facility"
                  checked={collectionMethod === "facility"}
                  onChange={(e) => setCollectionMethod(e.target.value as "home" | "facility")}
                  className="mr-2"
                />
                Tại cơ sở
              </label>
            </div>
          </div>

          {/* Service Selection */}
          <div>
            <label className="block text-sm font-medium text-gray-700 mb-2">
              Chọn dịch vụ <span className="text-red-500">*</span>
            </label>
            <select
              value={service.serviceId}
              disabled
              className="w-full px-3 py-2 border border-gray-300 rounded-lg bg-gray-100"
            >
              <option value={service.serviceId}>
                {service.serviceName} ({service.sampleCount} mẫu)
              </option>
            </select>
          </div>

          {/* Customer Information */}
          <div>
            <h3 className="text-lg font-medium text-gray-900 mb-4">Thông tin người yêu cầu</h3>
            <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
              <div>
                <label className="block text-sm font-medium text-gray-700 mb-1">
                  Họ và tên <span className="text-red-500">*</span>
                </label>
                <input
                  type="text"
                  value={customerInfo.fullName}
                  onChange={(e) => setCustomerInfo((prev) => ({ ...prev, fullName: e.target.value }))}
                  className="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-teal-500"
                  required
                />
              </div>
              <div>
                <label className="block text-sm font-medium text-gray-700 mb-1">
                  Ngày tháng năm sinh <span className="text-red-500">*</span>
                </label>
                <input
                  type="date"
                  value={customerInfo.dateOfBirth}
                  onChange={(e) => setCustomerInfo((prev) => ({ ...prev, dateOfBirth: e.target.value }))}
                  className="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-teal-500"
                  required
                />
              </div>
              <div>
                <label className="block text-sm font-medium text-gray-700 mb-1">
                  Giới tính <span className="text-red-500">*</span>
                </label>
                <select
                  value={customerInfo.gender}
                  onChange={(e) => setCustomerInfo((prev) => ({ ...prev, gender: e.target.value }))}
                  className="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-teal-500"
                >
                  <option value="Nam">Nam</option>
                  <option value="Nữ">Nữ</option>
                </select>
              </div>
              <div>
                <label className="block text-sm font-medium text-gray-700 mb-1">
                  Số điện thoại <span className="text-red-500">*</span>
                </label>
                <input
                  type="tel"
                  value={customerInfo.phoneNumber}
                  onChange={(e) => setCustomerInfo((prev) => ({ ...prev, phoneNumber: e.target.value }))}
                  className="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-teal-500"
                  required
                />
              </div>
            </div>
            <div className="mt-4">
              <label className="block text-sm font-medium text-gray-700 mb-1">
                Hình ảnh chứng từ <span className="text-red-500">*</span>
              </label>
              <input
                type="file"
                accept="image/*"
                onChange={(e) => handleFileUpload(0, e.target.files?.[0] || null, "customer")}
                className="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-teal-500"
              />
              <p className="text-xs text-gray-500 mt-1">Không có tập tin nào được chọn</p>
            </div>
          </div>

          {/* Sample Providers */}
          <div>
            <h3 className="text-lg font-medium text-gray-900 mb-4">Thông tin người xét nghiệm</h3>
            {sampleProviders.map((provider, index) => (
              <div key={index} className="bg-gray-50 rounded-lg p-4 mb-4">
                <div className="grid grid-cols-1 md:grid-cols-3 gap-4 mb-4">
                  <div>
                    <label className="block text-sm font-medium text-gray-700 mb-1">
                      Họ tên <span className="text-red-500">*</span>
                    </label>
                    <input
                      type="text"
                      value={provider.fullName}
                      onChange={(e) => handleSampleProviderChange(index, "fullName", e.target.value)}
                      className="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-teal-500"
                      required
                    />
                  </div>
                  <div>
                    <label className="block text-sm font-medium text-gray-700 mb-1">
                      Năm sinh <span className="text-red-500">*</span>
                    </label>
                    <input
                      type="date"
                      value={provider.dateOfBirth}
                      onChange={(e) => handleSampleProviderChange(index, "dateOfBirth", e.target.value)}
                      className="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-teal-500"
                      required
                    />
                  </div>
                  <div>
                    <label className="block text-sm font-medium text-gray-700 mb-1">
                      Giới tính <span className="text-red-500">*</span>
                    </label>
                    <select
                      value={provider.gender}
                      onChange={(e) => handleSampleProviderChange(index, "gender", e.target.value)}
                      className="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-teal-500"
                    >
                      <option value="Nam">Nam</option>
                      <option value="Nữ">Nữ</option>
                    </select>
                  </div>
                </div>
                <div className="grid grid-cols-1 md:grid-cols-2 gap-4 mb-4">
                  <div>
                    <label className="block text-sm font-medium text-gray-700 mb-1">
                      Mối quan hệ <span className="text-red-500">*</span>
                    </label>
                    <input
                      type="text"
                      value={provider.relationship}
                      onChange={(e) => handleSampleProviderChange(index, "relationship", e.target.value)}
                      placeholder="VD: Con, Cha, Mẹ..."
                      className="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-teal-500"
                      required
                    />
                  </div>
                  <div>
                    <label className="block text-sm font-medium text-gray-700 mb-1">
                      Loại mẫu <span className="text-red-500">*</span>
                    </label>
                    <select
                      value={provider.sampleType}
                      onChange={(e) => handleSampleProviderChange(index, "sampleType", e.target.value)}
                      className="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-teal-500"
                    >
                      <option value="CMND/CCCD/Passport">CMND/CCCD/Passport</option>
                      <option value="Chọn loại mẫu">Chọn loại mẫu</option>
                    </select>
                  </div>
                </div>
                <div>
                  <label className="block text-sm font-medium text-gray-700 mb-1">Hình ảnh chứng từ</label>
                  <input
                    type="file"
                    accept="image/*"
                    onChange={(e) => handleFileUpload(index, e.target.files?.[0] || null, "provider")}
                    className="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-teal-500"
                  />
                  <p className="text-xs text-gray-500 mt-1">Không có tập tin nào được chọn</p>
                </div>
              </div>
            ))}
          </div>

          {/* Terms and Conditions */}
          <div>
            <h3 className="text-lg font-medium text-gray-900 mb-4">Tôi xin cam kết</h3>
            <div className="space-y-2 text-sm text-gray-700 mb-4">
              <p>1. Tôi tự nguyện đề nghị xét nghiệm ADN và chấp nhận chi phí xét nghiệm.</p>
              <p>2. Những thông tin tôi đã khai trên đây là đúng sự thật và không thay đổi.</p>
              <p>3. Tôi không đề nghị nhà, người quen đến phiếu nhiều, làm mất trật tự.</p>
              <p>
                4. Những trường hợp sinh đôi, người ghép tủy, nhận mẫu, nếu không khai báo trung thực sẽ bị phạt gấp 2
                lần lệ phí đã nộp.
              </p>
              <p>
                5. Tôi đã đọc và chấp nhận các{" "}
                <button onClick={() => setShowTermsModal(true)} className="text-teal-600 hover:underline">
                  điều khoản của trung tâm GenUnity
                </button>{" "}
                và tôi đồng ý để Viện thực hiện các phân tích với các mẫu trên. Nếu vi phạm, tôi xin chịu hoàn toàn
                trách nhiệm trước pháp luật.
              </p>
            </div>
            <label className="flex items-center">
              <input
                type="checkbox"
                checked={acceptTerms}
                onChange={(e) => setAcceptTerms(e.target.checked)}
                className="mr-2"
                required
              />
              <span className="text-sm">* Tôi đồng ý với các điều khoản và cam kết nêu trên</span>
            </label>
          </div>

          {/* Submit Button */}
          <div className="flex justify-center">
            <button
              onClick={handleSubmit}
              disabled={loading || !acceptTerms}
              className="bg-teal-600 hover:bg-teal-700 disabled:bg-gray-400 text-white py-3 px-8 rounded-lg font-medium transition-colors"
            >
              {loading ? "Đang xử lý..." : "Gửi đăng ký"}
            </button>
          </div>
        </div>
      </div>

      {/* Terms Modal */}
      {showTermsModal && <TermsAndConditionsModal onClose={() => setShowTermsModal(false)} />}
    </div>
  )
}

```