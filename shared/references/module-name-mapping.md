# Module name mapping (Dotb UI labels → module key)

When the user mentions a module by **English or Vietnamese UI name** (navigation label, not the internal `J_Class` key), use this table to resolve the **canonical module name** used in code paths (`modules/<Name>/`, `BeanFactory::getBean('<Name>')`, vardefs, hooks).

## Source of truth in the consuming repo

Regenerated from:

- `custom/Extension/application/Ext/Language/en_us.moduleList.php` — `$app_list_strings['moduleList']` / `moduleListSingular`
- `custom/Extension/application/Ext/Language/vn_vn.moduleList.php` — same keys, Vietnamese labels

If a module is missing here, re-read those files in the consumer workspace (extensions may have been added since this snapshot).

## How the agent should use this

1. If the user says a label (e.g. “Schedules”, “Students”, “Lớp học”), **search this table** (English and Vietnamese columns) for a match.
2. Use the **Module key** column in all file paths and API calls.
3. If multiple rows could match fuzzy text, **ask the user** which module key they mean.
4. **SugarCRM defaults differ from Dotb labels:** e.g. `Contacts` is labeled “Students” / “Học viên”; `Meetings` is “Schedules” / “Cuộc học”; `Accounts` is “Corporates” / “Công ty”; `Cases` is “Feedback” / “Phản hồi”.

| Module key | English (list) | English (singular) | Vietnamese (list) | Vietnamese (singular) |
|------------|----------------|---------------------|-------------------|----------------------|
| `Accounts` | Corporates | Corporate | Công ty | Công ty |
| `ApprovalProcesses` | Approval Processes | Approval Process | Quy trình duyệt | Quy trình duyệt |
| `BMessage` | BMessage | BMessage | BMessage | BMessage |
| `Bugs` | EMS Support | EMS Support | Hỗ trợ EMS | Hỗ trợ EMS |
| `CJ_Forms` | Customer Journey Related Actions | Customer Journey Related Action | Customer Journey Related Actions | Customer Journey Related Action |
| `CJ_WebHooks` | Customer Journey Web Hooks | Customer Journey Web Hook | Customer Journey Web Hooks | Customer Journey Web Hook |
| `C_Attendance` | Attendance | Attendance | Điểm danh | Điểm danh |
| `C_Comments` | Comments | Comments | Bình luận | Bình luận |
| `C_Gallery` | Gallery | Gallery | Thư viện ảnh | Thư viện ảnh |
| `C_GalleryDetail` | GalleryDetails | GalleryDetail | GalleryDetails | GalleryDetail |
| `C_News` | News | News | Tin tức | Tin tức |
| `C_Parents` | Parents | Parent | Phụ huynh | Phụ huynh |
| `C_Rooms` | Rooms | Room | Phòng học | Phòng học |
| `C_SMS` | SMS | SMS | Tin nhắn SMS | Tin nhắn SMS |
| `C_Siblings` | Relationships | Relationship | Mối quan hệ | Mối quan hệ |
| `C_Teachers` | Teachers | Teacher | Giáo viên | Giáo viên |
| `C_Timesheet` | Admin Hours | Admin Hours | Giờ Admin | Giờ Admin |
| `Calendar` | Calendar | Calendar | Quản lý lịch | Quản lý lịch |
| `Calls` | Calls | Call | Cuộc gọi | Cuộc gọi |
| `Campaigns` | Campaigns | Campaign | Chiến dịch | Chiến dịch |
| `Cases` | Feedback | Feedback | Phản hồi | Phản hồi |
| `Chats` | Chat | Chat | Trò chuyện | Trò chuyện |
| `Contacts` | Students | Student | Học viên | Học viên |
| `Contracts` | Contracts | Contract | Hợp đồng | Hợp đồng |
| `DRI_SubWorkflow_Templates` | Customer Journey Stage Templates | Customer Journey Stage Template | Customer Journey Stage Templates | Customer Journey Stage Template |
| `DRI_SubWorkflows` | Customer Journey Stages | Customer Journey Stage | Customer Journey Stages | Customer Journey Stages |
| `DRI_Workflow_Task_Templates` | Customer Journey Activity Templates | Customer Journey Activity Template | Customer Journey Activity Templates | Customer Journey Activity Template |
| `DRI_Workflow_Templates` | Customer Journey Templates | Customer Journey Template | Customer Journey Templates | Customer Journey Template |
| `DRI_Workflows` | Customer Journeys | Customer Journey | Hành trình khách hàng | Hành trình khách hàng |
| `DataPrivacy` | Data Privacy | Data Privacy | Bảo mật dữ liệu | Bảo mật dữ liệu |
| `Documents` | Documents | Document | Tài liệu | Tài liệu |
| `EMS_Settings` | EMS Settings | EMS Settings | Cài đặt EMS | Cài đặt EMS |
| `Emails` | Emails | Email | Email | Email |
| `Forecasts` | Forecasts | Forecast | Dự báo | Dự báo |
| `Holidays` | Holidays | Holidays | Ngày nghỉ lễ | Ngày nghỉ lễ |
| `Home` | Home | Home | Trang chủ | Trang chủ |
| `J_AttDevice` | Attendance Devices | Attendance Device | Thiết bị điểm danh | Thiết bị điểm danh |
| `J_BankTrans` | Bank Transactions | Bank Transactions | Giao dịch ngân hàng | Giao dịch ngân hàng |
| `J_Budget` | Expense | Budget Plan | Phiếu chi | Phiếu chi |
| `J_CheckInOut` | Check In/Out Log | Check In/Out Log | Check In/Out Log | Check In/Out Log |
| `J_Class` | Classes | Class | Lớp học | Lớp học |
| `J_ClassStudents` | Class Students | Class Students | Class Students | Class Students |
| `J_Commission` | Commission | — | Hoa hồng | — |
| `J_ConfigInvoiceNo` | Config: E-Invoice | Config:InvoiceNo | Config: E-Invoice | Config: E-Invoice |
| `J_Discount` | Discounts | Discount | Chiết khấu | Chiết khấu |
| `J_Dish` | Dishes | Dish | Món ăn | Món ăn |
| `J_Gradebook` | Gradebooks | Gradebook | Bảng điểm | Bảng điểm |
| `J_GradebookConfig` | Gradebook Settings | Gradebook Setting | Cấu hình bảng điểm | Cấu hình bảng điểm |
| `J_GradebookDetail` | Gradebook Details | Gradebook Detail | Chi tiết bảng điểm | Chi tiết bảng điểm |
| `J_GradebookSettingGroup` | Gradebook Setting Groups | Gradebook Setting Group | Cấu hình nhóm Bảng điểm | Cấu hình nhóm Bảng điểm |
| `J_Health` | Health | Health | Sức khỏe | Sức khỏe |
| `J_ImportHistory` | Import/Export History | Import/Export History | Lịch sử nhập | Lịch sử nhập |
| `J_Inventory` | Inventory | Inventory | Kho hàng | Kho hàng |
| `J_Inventorydetail` | Inventory Details | Inventory Detail | Chi tiết kho | Chi tiết kho |
| `J_Invoice` | Invoices | Invoice | Hoá đơn | Hoá đơn |
| `J_Kindofcourse` | Kind Of Courses | Kind Of Course | Chương trình học | Chương trình học |
| `J_Loyalty` | Loyalty | Loyalty | Điểm tích luỹ | Điểm tích luỹ |
| `J_MenuItem` | — | — | Thực đơn | Thực đơn |
| `J_MenuPlanner` | Menu Planner | Menu Planner | Thực đơn | Thực đơn |
| `J_Metric` | Metric | Metric | Metric | Metric |
| `J_Omnichat` | Omnichat | Omnichat | — | — |
| `J_PTResult` | PT/Demo Results | PT/Demo Result | Kết quả PT/Demo | Kết quả PT/Demo |
| `J_ReferenceLog` | Reference Logs | Reference Log | Reference Logs | Reference Log |
| `J_School` | Schools | School | Trường học | Trường học |
| `J_Sponsor` | Discount/Sponsor Details | Discount/Sponsor Detail | Chi tiết giảm giá | Chi tiết giảm giá |
| `J_StudentSituations` | Student Situations | Student Situations | Quá trình học | Quá trình học |
| `J_Syllabus` | Syllabus | Syllabus | Giáo trình | Giáo trình |
| `J_Targetconfig` | KPI Settings | KPI Settings | Cấu hình KPI | Cấu hình KPI |
| `J_TeacherQE` | Teaching Performance | Teaching Performance | Hiệu suất giảng dạy | Hiệu suất giảng dạy |
| `J_Teachercontract` | Teacher Contracts | Teacher Contract | Hợp đồng GV | Hợp đồng GV |
| `J_VietQRConfig` | Bank Connections | Bank Connections | Kết nối Ngân hàng | Kết nối Ngân hàng |
| `J_Voucher` | Vouchers | Voucher | Mã giảm giá | Mã giảm giá |
| `J_Workpermit` | Work permit | Work permit | Giấy phép lao động | Giấy phép lao động |
| `KBContents` | News Article | News Article | Cơ sở tri thức | Cơ sở tri thức |
| `Leads` | Leads | Lead | HV Tiềm năng | HV Tiềm năng |
| `M_Activities` | Activities | Activities | Hoạt động học | Hoạt động học |
| `M_MetrikalAlert` | MetrikalAlert | MetrikalAlert | MetrikalAlert | MetrikalAlert |
| `Meetings` | Schedules | Schedule | Cuộc họp | Cuộc họp |
| `Notes` | Notes | Note | Ghi chú | Ghi chú |
| `NotificationComposer` | Notification Composers | Notification Composer | Quản lý soạn thông báo | Quản lý soạn thông báo |
| `Opportunities` | Opportunities | Opportunity | Cơ hội | Cơ hội |
| `OutboundEmail` | Email Settings | Email Setting | Cài đặt email | Cài đặt email |
| `ProductCategories` | Product Categories | Product Category | Danh mục sản phẩm | Danh mục sản phẩm |
| `ProductTemplates` | Product Template | Product Template | Mẫu sản phẩm | Mẫu sản phẩm |
| `ProductTypes` | Product Types | Product Type | Loại sản phẩm | Loại sản phẩm |
| `Products` | Order Items | Order Item | Chi tiết đơn hàng | Chi tiết đơn hàng |
| `ProspectLists` | Target Lists | Target List | Danh sách mục tiêu | Danh sách mục tiêu |
| `Prospects` | Targets | Target | KH Mục tiêu | KH Mục tiêu |
| `Quotes` | Sale Orders | Sale Order | Đơn hàng | Đơn hàng |
| `Receipts` | Receipts | Receipts | Phiếu thu | Phiếu thu |
| `Reports` | Reports | Report | Báo cáo | Báo cáo |
| `RevenueLineItems` | Revenue Line Items | Revenue Line Item | Mục hàng doanh thu | Mục hàng doanh thu |
| `Tags` | Tags | Tag | Gắn Thẻ | Gắn Thẻ |
| `Tasks` | — | — | Công việc | Công việc |
| `fte_UsageTracking` | UsageTracking | UsageTracking | UsageTracking | UsageTracking |
| `hed_Balance` | Balances | Balance | Số dư | Số dư |
| `hed_Term` | Terms | Term | Học kỳ | Học kỳ |
| `pmse_BpmFlow` | Approval Flows | Approval Flow | Luồng duyệt | Luồng duyệt |
| `pmse_Business_Rules` | Process Business Rules | Process Business Rule | BPMN: Quy tắc KD | BPMN: Quy tắc KD |
| `pmse_Emails_Templates` | Process Email Templates | Process Email Template | BPMN: Mẫu email | BPMN: Mẫu email |
| `pmse_Inbox` | Processes | Process | BPMN: Lịch sử quy trình | BPMN: Lịch sử quy trình |
| `pmse_Project` | Process Definitions | Process Definition | BPMN: Định nghĩa quy trình | BPMN: Định nghĩa quy trình |
