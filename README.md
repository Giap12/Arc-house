# Arc House

Đăng việc (bounty) bằng USDC, trả theo cột mốc, có bằng chứng. Chạy trên Arc Testnet. **Chưa kiểm toán, chỉ dùng testnet.**

## Chạy thử
Mở `index.html` bằng trình duyệt, hoặc bật GitHub Pages (Settings → Pages → Deploy from a branch → `main` / root). Khi chưa có địa chỉ hợp đồng, trang hiện dữ liệu mẫu.

## Kết nối hợp đồng thật
Mở mục **Cấu hình** trong trang và nhập: địa chỉ `BountyEscrow`, RPC URL và explorer của Arc Testnet, block triển khai. Cấu hình lưu trong trình duyệt, không đưa lên GitHub.

## Hợp đồng phải khớp ABI của frontend
`BountyEscrow` cần có:
- `createBounty((address reviewer,uint64 deadline,bool openClaim,bytes32 category,string metadataURI,uint256[] milestoneAmounts)) returns (uint256)`
- `getBounty(uint256)` trả `(poster, reviewer, worker, deadline, status, openClaim, milestoneCount, category, metadataURI)`
- `getMilestones(uint256)` trả mảng `(amount, dueAt, submittedAt, revisions, disputedAt, status, deliverableHash, deliverableURI)`
- Sự kiện `BountyCreated(uint256 indexed id, address indexed poster, address reviewer, uint256 total, uint64 deadline, bytes32 category, string metadataURI)`
- Các hàm: `apply`, `claim`, `assignWorker`, `cancel`, `submitMilestone`, `approveMilestone`, `requestChanges`, `autoRelease`, `raiseDispute`, `resolveStaleDispute`, `refundExpired` (chữ ký như đặc tả MVP).

Nếu bạn đổi chữ ký trong Solidity, hãy sửa mảng `ABI` trong `index.html` cho khớp.

## Lưu ý
- Số chữ số thập phân của USDC được đọc từ hợp đồng USDC khi kết nối ví.
- Hash bản giao hiện là `keccak256` của link nộp (bản MVP). Muốn bằng chứng chặt hơn, băm cả manifest.
- Không bao giờ đưa khóa riêng hoặc file `.env` lên GitHub.
