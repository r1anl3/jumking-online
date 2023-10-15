
**ĐẠI HỌC QUỐC GIA TP.HCM**

TRƯỜNG ĐẠI HỌC CÔNG NGHỆ THÔNG TIN



**ĐỒ ÁN MÔN HỌC**

**LẬP TRÌNH MẠNG CĂN BẢN**

**JUMPKING ONLINE WITH UNITY**


**NHÓM 2**



`                                 `Giáo viên hướng dẫn – Đặng Lê Bảo Chương

Sinh viên thực hiện:

21522797 – Lê Huỳnh Quang Vũ













**
# Mục lục
[I.**	**Định nghĩa	3****](#_toc138341003)**

[**1.**	**Giới thiệu**	3](#_toc138341004)

[**a.**	**Tổng quan về trò chơi**	3](#_toc138341005)

[**b.**	**Giới thiệu về trò chơi**	3](#_toc138341006)

[**2.**	**Gameplay**	4](#_toc138341007)

[**II.**	**Triển khai	4****](#_toc138341008)

[**1.**	**Tính năng**	4](#_toc138341009)

[Một số hình ảnh minh họa:	5](#_toc138341010)

[**2.**	**Mô hình phân rã chức năng (BFD)**	6](#_toc138341011)

[**3.**	**Cấu trúc gói tin**	7](#_toc138341012)

[**a.**	**Client đến Server**	7](#_toc138341013)

[**b.**	**Server đến Client**	8](#_toc138341014)

[**4.**	**Network stack**	9](#_toc138341015)

[**III.**	**Phân chia công việc	11****](#_toc138341016)



**

1. <a name="_toc138341003"></a>**Định nghĩa**
   1. <a name="_toc138341004"></a>**Giới thiệu**
      1. <a name="_toc138341005"></a>**Tổng quan về trò chơi**
- **Jumpking** là một trò chơi trực tuyến được phát bởi Nexile và được phát hành trên nền tảng Steam vào ngày 09/06/2019.
- Trong trò chơi, bản đồ của game ở dạng thẳng đứng. Do đó, người chơi phải cẩn trọng trong từng bước nhảy, tránh việc bị ngã từ trên cao.
  1. <a name="_toc138341006"></a>**Giới thiệu về trò chơi**
- **Nhân vật:** King và Knight.


*Figure 1: Knight character*

*Figure 2: King character*


- **Bản đồ:** Redcrow Woods.


*Figure 3: Redcrow Woods map*

1. <a name="_toc138341007"></a>**Gameplay** 
- Số lượng người chơi trong một phòng: 2.
- Sau khi nhập mã phòng, người chơi sẽ được đưa vào lobby, host sẽ có quyền bắt đầu trò chơi.
- Khi vào game, người chơi sẽ được spawn vào bản đồ game.
- Bản đồ sẽ bao gồm: 
  - Block.
  - Building object.
  - Npc.
- Người chơi nào lên được đỉnh tháp trước sẽ là người chiến thắng.
1. <a name="_toc138341008"></a>**Triển khai**
   1. <a name="_toc138341009"></a>**Tính năng**
- Tạo phòng chơi với số lượng người chơi là 2 (1 host).
- Tham gia phòng chơi bằng code (1 client).
- Tạo lobby, relay. 
- Tạm dừng game.
### <a name="_toc138341010"></a>**Một số hình ảnh minh họa:** 

*Figure 4: Create and Join game room*

*Figure 5: Game lobby*


*Figure 6: Pause game*

1. <a name="_toc138341011"></a>**Mô hình phân rã chức năng (BFD)**


*Figure 7: Game BFD*

1. <a name="_toc138341012"></a>**Cấu trúc gói tin**
- **Thư viện:** *Unity Netcode*, *Unity Relay*, *Unity Lobby*
- **Chức năng:** 
  - Đồng bộ vị trí *Player*.
  - Đồng bộ *Animation*.
  - Tạo kết nối.
  - Tạo phòng chờ.
  - Kết nối *Player* vào *Server* thông qua *Unity Services* (Global Hosting).
    1. <a name="_toc138341013"></a>**Client đến Server**

|<p><a name="_hlk137216113"></a>**Control Message:** </p><p>Connect/Disconnect/Data/</p>|**Length:** 6144 bytes|
| :-: | :-: |
|**Sender:** Client|**Receiver:** Server|
|**Body**||


|**Packet**|||||
| :-: | :- | :- | :- | :- |
|NetworkObjectId|Position|Rotation|Scale|State|


*Source: NetworkObject.cs*

- **Giải nghĩa**:
  - *Connect*: người chơi yêu cầu kết nối vào Server.
  - *Disconnect*: người chơi yêu cầu rời Server.
  - *Data*: người chơi gửi thông tin trong Packet.
  - *NetworkObjectId*: Id của người chơi.
  - *Position*: Vị trí người chơi hiện tại.
  - *Rotation*: Trạng thái xoay của người chơi, cố định giá trị (0,0,0).
  - *Scale*: Trạng thái lật của người chơi, di chuyển sang phải (1,1,1), di chuyển sang phải (-1,1,1).
  - *State*: hoạt ảnh hiện tại, gồm 5 hoạt ảnh {idle, run, hold, jump, fall}.
    1. <a name="_toc138341014"></a>**Server đến Client** 

|<p>**Control Message:** </p><p>IsServer/IsClient/IsListening/</p><p>IsApproved</p>|**Length:** 6144 bytes|
| :-: | :-: |
|**Sender:** Server|**Receiver:** Client|
|**Body**||


|**Packet**||||
| :-: | :- | :- | :- |
|LocalClientId|NetworkPrefabs|NetworkTransform|NetworkAnimator|

- **Giải nghĩa:**
  - *IsServer*: kiểm tra xem có phải là Server.
  - *IsClient*: kiểm tra nếu là người chơi.
  - Trong đồ án này, Host sẽ là Server đồng thời cũng là Client.
  - *IsListening*: kiểm tra xem Server có đang mở kết nối.
  - *IsApproved*: kiểm tra người chơi có được chấp nhận.
  - *LocalClientId*: Id của người chơi, được cấp bởi Server thông qua NetworkObjectId.
  - *NetworkPrefabs*: Nhân vật của người chơi.
  - *NetworkTransform*: Vị trí người chơi được trả về, bao gồm:
    - *Position*
    - *Rotation*
    - *Scale*
  - *NetworkAnimator*: Hoạt ảnh nhân vật được trả về, bao gồm:
    - *Sprite*
    - *Animation*
- Đối với mỗi lần gửi, Server sẽ gửi cho (n – 1) gói tin cho (n) người chơi. 




1. <a name="_toc138341015"></a>**Network stack** 

***(Xem ở trang kế tiếp …)***

1. <a name="_toc138341016"></a>**Phân chia công việc** 

|**MSSV**|**Họ tên**|**Công việc**|**% Công việc**|
| :- | :- | :- | :- |
|21522797|Lê Huỳnh Quang Vũ|<p>- Thiết kế gameplay, lập trình logic game.</p><p>- Thiết kế mạng, lập trình mạng.</p><p>- Thuyết trình.</p><p>- Làm báo báo.</p>|100%|

***Video demo*:** <https://drive.google.com/file/d/1auDn2UTejUFmqPWJiGwy-PbYFSz_Qs_t/view?usp=sharing>

***Source code: [JumpKing-Online/Assets at master · tuitenrein/JumpKing-Online (github.com)](https://github.com/tuitenrein/JumpKing-Online/tree/master/Assets)***

10

