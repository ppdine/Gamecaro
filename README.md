# Game Caro

## Yêu cầu hệ thống
- Java 23: https://www.oracle.com/cis/java/technologies/downloads/#java23
- MySQL: https://dev.mysql.com/downloads/installer/
- IntelliJ IDEA: https://www.jetbrains.com/idea/download/?section=windows
- MySql JDBC Driver: https://dbschema.com/jdbc-drivers/MySqlJdbcDriver.zip

## Công nghệ sử dụng
- **Socket TCP/UDP**: Kết nối và giao tiếp giữa client và server.
- **Multistream**: Xử lý nhiều kết nối từ client trên server.
- **Java Swing**: Tạo giao diện người dùng cho client.
- **MySQL**: Lưu trữ thông tin người chơi và dữ liệu game.

## Cấu trúc dự án
Gồm 2 phần chính: client và server.
### Client
- **controller**: Các lớp điều khiển cho client `Client` và `SocketHandle`.
- **model**: Định nghĩa các đối tượng dữ liệu  `Point`, `User`, và `XOButton`.
- **view**: Các form giao diện:
  - `LoginFrm`: Giao diện đăng nhập.
  - `RegisterFrm`: Giao diện đăng ký.
  - `GameClientFrm`: Giao diện chính khi vào game.
  - `FriendListFrm`, `FriendRequestFrm`: Các form liên quan đến bạn bè.
  - ......

### Server
- **controller**: Các lớp điều khiển server `Room`, `Server`, `ServerThread` và `ServerThreadBus`.
- **dao**: Kết nối và tương tác với MySQL. `Dao`, `UserDAO`
- **model**: Định nghĩa đối tượng `User`.
- **sql**: File `create_db.sql` chứa script tạo cơ sở dữ liệu.
- **view**: `Admin` giao diện quản trị để quản lý người chơi.

## Chức năng
- Đăng ký và đăng nhập tài khoản (dùng MySQL để lưu trữ thông tin).
- Quản lý bạn bè: Thêm bạn bè và xem danh sách bạn bè.
- Chơi với máy, tạo phòng và tham gia phòng có sẵn.
- Call, gửi và nhận tin nhắn trong phòng chơi.
- Quản trị: Quản lý người dùng, xem danh sách phòng và luồng.
- Bảng xếp hạng: Xếp hạng người chơi theo số trận thắng.
- Server hỗ trợ nhiều kết nối đồng thời bằng cách sử dụng đa luồng.

## Hướng dẫn cài đặt
1. Clone repository:

    ```bash
    https://github.com/ppdine/Gamecaro.git
    ```

2. Cấu hình MySQL:
    - Tạo một database trong MySQL (ví dụ: `carodb`).
    - Chạy các lệnh SQL dưới đây trong file `carodb.sql`.<br><br>
      ```sql
      CREATE TABLE user (
      ID int AUTO_INCREMENT PRIMARY KEY,
      username varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_as_cs UNIQUE,
      password varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_as_cs,
      nickname varchar(255),
      avatar varchar(255),
      numberOfGame int DEFAULT 0,
      numberOfWin int DEFAULT 0,
       numberOfDraw int DEFAULT 0,
      isOnline int DEFAULT 0,
      isPlaying int DEFAULT 0
      );
  
      CREATE TABLE friend (
      ID_User1 int NOT NULL,
      ID_User2 int NOT NULL,
      FOREIGN KEY (ID_User1) REFERENCES user(ID),
      FOREIGN KEY (ID_User2) REFERENCES user(ID),
      CONSTRAINT PK_friend PRIMARY KEY (ID_User1, ID_User2)
      );

      CREATE TABLE banned_user (
      ID_User int PRIMARY KEY NOT NULL,
      FOREIGN KEY (ID_User) REFERENCES user(ID)
      );
3. Import dự án vào IntelliJ
    - Thêm MySql JDBC Driver vào modules
    - Cấu hình kết nối MySQL ở `Dao.java`. <br><br>
      ```java
      final String DATABASE_NAME = "database_name";
      ...
      final String JDBC_USER = "username";
      final String JDBC_PASSWORD = "password";
      
