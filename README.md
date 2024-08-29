# thật đáng sợ

Ứng dụng khách web dành cho [Genymobile/scrcpy][scrcpy] và hơn thế nữa.

## Yêu cầu

Trình duyệt phải hỗ trợ các công nghệ sau:
* WebSockets
* Tiện ích mở rộng nguồn phương tiện và giải mã h264;
* WebWorker
* WebAssembly

máy chủ:
* Node.js v10+
* nút-gyp ([cài đặt](https://github.com/nodejs/node-gyp#installation))
* Tệp thực thi `adb` phải có sẵn trong biến môi trường PATH

Thiết bị:
* Android 5.0+ (API 21+)
* Đã bật [gỡ lỗi adb](https://developer.android.com/studio/command-line/adb.html#Enabling)
* Trên một số thiết bị, bạn cũng cần bật
[tùy chọn bổ sung](https://github.com/Genymobile/scrcpy/issues/70#issuecomment-373286323)
để điều khiển nó bằng bàn phím và chuột.

## Xây dựng và bắt đầu

Đảm bảo bạn đã cài đặt [node.js](https://nodejs.org/en/download/),
[node-gyp](https://github.com/nodejs/node-gyp) và
[công cụ xây dựng](https://github.com/nodejs/node-gyp#installation)
``` vỏ
bản sao git https://github.com/NetrisTV/ws-scrcpy.git
cd ws-srcpy

## Để có phiên bản ổn định, hãy tìm thẻ mới nhất và chuyển sang thẻ đó:
# thẻ git -l
# git thanh toán vX.Y.Z

cài đặt npm
bắt đầu npm
```

## Các tính năng được hỗ trợ

###Android

#### Truyền màn hình
[Phiên bản] [phân nhánh] đã sửa đổi của [Genymobile/scrcpy] [scrcpy] được sử dụng để phát trực tuyến
H264-video, sau đó được giải mã bằng một trong các bộ giải mã đi kèm:

##### Trình phát Mse

Dựa trên [bộ chuyển đổi xevokk/h264] [bộ chuyển đổi xevokk/h264].
Video HTML5.<br>
Yêu cầu [API nguồn phương tiện] [MSE] và `video/mp4; codecs="avc1.42E01E"`
[hỗ trợ][isTypeĐược hỗ trợ]. Tạo các thùng chứa mp4 từ NALU, nhận được từ một
thiết bị, sau đó cung cấp chúng cho [MediaSource][MediaSource]. Về lý thuyết, nó có thể được sử dụng
tăng tốc phần cứng.

##### Người chơi Broadway

Dựa trên [mbebenita/Broadway][broadway] và
[131/h264-live-player][h264-live-player].<br>
Phần mềm giải mã video được biên dịch thành mô-đun wasm.
Yêu cầu hỗ trợ [WebAssembly][wasm] và tốt nhất là [WebGL][webgl].

##### Máy nghe nhạc TinyH264

Dựa trên [udevbe/tinyh264][tinyh264].<br>
Phần mềm giải mã video được biên dịch thành mô-đun wasm. Một phiên bản cập nhật nhẹ của
[mbebenita/Broadway][broadway].
Yêu cầu hỗ trợ [WebAssembly][wasm], [WebWorkers][workers], [WebGL][webgl].

##### Trình phát WebCodecs

Việc giải mã được thực hiện bằng bộ giải mã phương tiện (phần mềm/phần cứng) tích hợp trong trình duyệt.
Yêu cầu hỗ trợ [WebCodecs][webcodecs]. Hiện tại chỉ có tại
[Chromium](https://www.chromestatus.com/feature/5669293909868544) và các dẫn xuất.

####Điều khiển từ xa
* Sự kiện chạm (bao gồm cảm ứng đa điểm)
* Mô phỏng cảm ứng đa điểm: <kbd>CTRL</kbd> để bắt đầu với tâm ở giữa
màn hình, <kbd>SHIFT</kbd> + <kbd>CTRL</kbd> để bắt đầu với tâm tại
điểm hiện tại
* Bánh xe chuột và bàn di chuột cuộn dọc/ngang
* Ghi lại các sự kiện bàn phím
* Chèn văn bản (chỉ ASCII)
* Sao chép vào/từ clipboard của thiết bị
* Thiết bị "xoay"

#### Đẩy tập tin
Kéo và thả tệp APK để đẩy tệp đó vào thư mục `/data/local/tmp`. bạn có thể
cài đặt thủ công từ thiết bị đầu cuối [xtermjs/xterm.js] đi kèm
trình giả lập (xem bên dưới).

#### Vỏ từ xa
Điều khiển thiết bị của bạn từ `adb shell` trong trình duyệt của bạn.

#### Gỡ lỗi trang web/WebView
[/docs/Devtools.md](/docs/Devtools.md)

#### Danh sách tập tin
* Liệt kê các tập tin
* Tải tệp lên bằng cách kéo và thả
* Tải tập tin

###iOS

***Tính năng thử nghiệm***: *không được xây dựng theo mặc định*
(xem [bản dựng tùy chỉnh](#custom-build))

#### Truyền màn hình

Yêu cầu [ws-qvh][ws-qvh] có sẵn trong `PATH`.

#### Máy chủ MJPEG

Kích hoạt `USE_WDA_MJPEG_SERVER` trong tệp cấu hình bản dựng
(xem [bản dựng tùy chỉnh](#custom-build)).

Cách khác để truyền phát nội dung màn hình. Nó không
yêu cầu phần mềm bổ sung như `ws-qvh`, nhưng có thể yêu cầu nhiều tài nguyên hơn tùy theo
khung được mã hóa dưới dạng hình ảnh jpeg.

####Điều khiển từ xa

Để điều khiển thiết bị, chúng tôi sử dụng [appium/WebDriverAgent][WebDriverAgent].
Chức năng giới hạn ở:
* Chạm đơn giản
* Cuộn
* Bấm nút Home

Đảm bảo bạn đã [thiết lập WebDriverAgent] đúng cách (https://appium.io/docs/en/drivers/ios-xcuitest-real-devices/).
Dự án WebDriverAgent nằm trong `node_modules/appium-webdriveragent/`.

Bạn có thể muốn bật ``AssistiveTouch'' trên thiết bị của mình: ``Cài đặt/Chung/Trợ năng''.

## Bản dựng tùy chỉnh

Bạn có thể tùy chỉnh dự án trước khi xây dựng bằng cách ghi đè
[cấu hình mặc định](/webpack/default.build.config.json) trong
[build.config.override.json](/build.config.override.json):
* `INCLUDE_APPL` - bao gồm mã để theo dõi và kiểm soát thiết bị iOS
* `INCLUDE_GOOG` - bao gồm mã để theo dõi và kiểm soát thiết bị Android
* `INCLUDE_ADB_SHELL` - [vỏ từ xa](#remote-shell) dành cho thiết bị Android
([xtermjs/xterm.js][xterm.js], [Tyriar/node-pty][node-pty])
* `INCLUDE_DEV_TOOLS` - [dev tools](#debug-webpageswebview) cho các trang web và
lượt xem web trên thiết bị Android
* `INCLUDE_FILE_LISTING` - [quản lý tệp] tối giản(#file-listing)
* `USE_BROADWAY` - bao gồm [Broadway Player](#broadway-player)
* `USE_H264_CONVERTER` - bao gồm [Mse Player](#mse-player)
*"# ws-scrcpy" 
