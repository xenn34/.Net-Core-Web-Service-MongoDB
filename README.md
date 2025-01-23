
1. Cách cài đặt và sử dụng MongoDB
   - Cài đặt MongoDB từ trang chủ MongoDB.
   - Cài đặt Mongo Compass để quản lý cơ sở dữ liệu.
   - Tạo database và thêm các document mẫu
2. Cài đặt MongoDB Drive từ NuGet Package Manager để thêm MongoDB Driver vào dự án.
3. Biết được cấu trúc dự án:
   - Thêm thư mục Models và tạo các model:
   - Book (định nghĩa cấu trúc dữ liệu cho sách).
   - BookStoreDatabaseSettings (để cấu hình database).
   - Sử dụng DI (Dependency Injection) để lấy thông tin cấu hình từ appsettings.json.

4. Tạo các Service cho tác vụ CRUD:
   - Tạo thư mục Services.
   - Thêm BooksService để triển khai các phương thức: Lấy danh sách tất cả sách, lấy sách theo id, thêm mới sách, cập nhật sách, xóa sách.
5. Tạo Controller:
   - Thêm BooksController với các endpoint:
     - GET /api/books: Lấy danh sách sách.
     - GET /api/books/{id}: Lấy sách theo id.
     - POST /api/books: Thêm mới sách.
     - PUT /api/books/{id}: Cập nhật sách.
     - DELETE /api/books/{id}: Xóa sách.

6. Cập nhật Program.cs:
   - Cấu hình Dependency Injection cho BookStoreDatabaseSettings và BooksService.
   - Sử dụng Swagger để tự động tạo tài liệu API.

7. **Chạy ứng dụng**:
   - Trust SSL certificate khi được hỏi.
   - Truy cập Swagger theo địa chỉ: https://localhost:7009/swagger/index.htm

Ghi chú:
- Đối với Dependency Injection (DI):
  - Sử dụng DI để cấu hình BookStoreDatabaseSettings, giúp giảm sự phụ thuộc chặt chẽ và dễ dàng quản lý cấu hình.
  - Ví dụ: 
    builder.Services.Configure<BookStoreDatabaseSettings>(
        builder.Configuration.GetSection("BookStoreDatabase"));

- Quản lý MongoDB Collection:
  - MongoClient và IMongoCollection được sử dụng để thao tác với MongoDB:

    var mongoClient = new MongoClient(bookStoreDatabaseSettings.Value.ConnectionString);
    var mongoDatabase = mongoClient.GetDatabase(bookStoreDatabaseSettings.Value.DatabaseName);
    _booksCollection = mongoDatabase.GetCollection<Book>(bookStoreDatabaseSettings.Value.BooksCollectionName);

- Swagger:
  - Swagger là công cụ mạnh mẽ để kiểm tra API trực tiếp và tự động tạo tài liệu.
  - Sau khi thêm builder.Services.AddSwaggerGen(); vào Program.cs, Swagger sẽ:
    1. Hiển thị danh sách các endpoint API.
    2. Cho phép bạn kiểm tra các endpoint trực tiếp trong trình duyệt.
    3. Cung cấp giao diện thân thiện để xem thông tin như tham số, response, và request mẫu.
  - Để sử dụng Swagger, em cần bật nó trong môi trường phát triển:
    if (app.Environment.IsDevelopment())
    {
        app.UseSwagger();
        app.UseSwaggerUI();
    }
  - Giao diện Swagger UI có thể truy cập qua URL được cung cấp khi chạy ứng dụng.

- JSON Serialization:
  - Tùy chỉnh JsonSerializerOptions để đảm bảo dữ liệu trả về không bị thay đổi định dạng tên thuộc tính:
    builder.Services.AddControllers().AddJsonOptions(
        options => options.JsonSerializerOptions.PropertyNamingPolicy = null);
