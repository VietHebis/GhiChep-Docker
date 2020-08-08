  #  GhiChep-Docker
 
Describe a learning processing about Docker 

  ###  Container
 - Liệt kê số lượng container đang chạy (run) trên host:
	
	`docker ps`

 - Thêm tham số (-a) để liệt kê tất cả các container đang có trên host (bao gồm cả đang chạy và dừng):
	
	`docker ps -a`

 - Tạo một container từ image, nếu image không có sẵn trên local host thì docker sẽ tìm trên docker hub image phiên bản latest để tải về và chạy:
 
	`docker run ubuntu`
￼
 - Đặt tên cho một container khi khởi tạo, thêm tham số (--name) 
	
	`docker run --name ubuntu_test ubuntu`

 - Tạo một docker và sử dụng terminal của docker
	
	`docker run -it --name ubuntu_test1 ubuntu /bin/bash`
    
    -> Khi sử dụng tham số (-it) thì khi thoát terminal của docker bằng exit, docker sẽ bị dừng.
 
 - Chỉ định chạy một docker dưới nền như một deamon. 
	
	`docker run -d ubuntu /bin/bash`

 - Vào terminal của một container đang chạy
	
	`docker exec -it ubuntu_test1 /bin/bash`

    -> Thoát ra khỏi một container khi đang sử dụng terminal bằng cách: - Gõ exit - Nhấn tổ hợp phím Ctl+P và Ctl+Q

 - Xem thông tin chi tiết về một container bằng lệnh:
	
	`docker inspect ubuntu_test1`

 - Xóa một container ở trạng thái Exited
	
	`docker rm ubuntu_test`

 - Xóa một container ở trạng thái Up thì thêm tham số (-f)
	
	`docker rm -f ubuntu_test1`

 - Lệnh xóa tất cả các container trên host: 
	
	`docker rm -f $(docker ps -aq)`

 - Commit một container thành image:
	
	`docker commit ubuntu_test1 tannt/ubuntu:latest`

 - Khởi động/dừng một container
	
	`docker [restart|start|stop] ubuntu_test1`

 - Cách tra cứu cú pháp các command:
	
	`docker [command] --help`


 ### IMAGE
 - Tìm kiếm một image từ docker hub, ví dụ tìm kiếm image về apache:
	
	`docker search apache`

 - Tải một image về máy:
 
	`docker pull httpd`

 - Liệt kê tất cả các image đang có trên local:
 
	`docker images`

 - Xem chi tiết thông tin về image:
 
	`docker image inspect httpd`

 - Xem lịch sử các commit của image:
 
	`docker history httpd`

 - Xóa một image:
 
	`docker rmi httpd`
	
     -> Xóa tất cả các image bằng lệnh
     
     `docker rmi -f $(docker images -qa)`
    
      -> Khi image đang được sử dụng để run một container thì muốn xóa, bạn cần thêm tham số (-f)

 - Đổi lại tag của một image:
 
	`docker tag httpd httpd:1.0`
	
    -> httpd là image đang có. httpd:1.0 là tên mới của image có phiên bản 1.0, nếu không điền thì được mặc định là latest
    -> có thể chỉ định thêm repository sẽ upload image, tôi sử dụng repository là test: 
    
    `docker tag httpd longtran/httpd:1.0`

 - Cách tra cứu cú pháp các command về image:
 
	`docker [command] --help`

 - Ví dụ:
 
	`docker image --help`
	
	`docker image pull --help`


 ### NETWORK
 - Liệt kê các network đang có của host
 
	`docker network ls`

 - Thông tin chi tiết về một network mặc định bridge
 
	`docker network inspect bridge`

 - Tạo một bridge network với subnet chỉ định
 
	`docker network create --driver=bridge --subnet=192.168.100.0/24 my-net1`

 - Chạy một container với network chỉ định
 
	`docker run --network=my-net1 -d httpd`

 - Cách tra cứu cú pháp các command về image
 
	`docker network [command] --help`

 - Trên host, cần đăng nhập vào docker hub để có thể push được image. Chạy lệnh sau và nhập username/password tương ứng.
 
	`docker login`

 - Giả sử bạn đang có một image về mysql, bạn cần push lên docker hub thì trước tiên phải đổi lại như sau:
 
	`docker tag [image_name] [username_hub]/[image_name]:[tag]`

 - Ví dụ:
 
	`docker tag apache_test longtran/apache_test:1.0`

 - Nếu bạn không gắn tag thì mặc định là latest. Bây giờ bạn push lên docker hub của bạn
 
	`docker push [username_hub]/[image_name]:[tag]`

 - Ví dụ:
 
	`docker push longtran/apache_test:1.0`

 - Note: 
    -> Trong trường hợp image_name chưa có trên docker hub thì lúc bạn push lên, sẽ tự tạo ra một repository mới
    -> Khi một người dùng muốn sử dụng image vừa được bạn tải lên, sẽ sử dụng lệnh pull:
	
	`docker pull longtran/apache_test:1.0`



 ### REGISTRY
 - Trên host, cần đăng nhập vào docker hub để có thể push được image. Chạy lệnh sau và nhập username/password tương ứng.
 
	`docker login`

 - Giả sử bạn đang có một image về mysql, bạn cần push lên docker hub thì trước tiên phải đổi lại như sau:
 
	`docker tag [image_name] [username_hub]/[image_name]:[tag]`

 - Ví dụ:
 
	`docker tag apache_test longtran/apache_test:1.0`

 - Nếu bạn không gắn tag thì mặc định là latest. Bây giờ bạn push lên docker hub của bạn
 
	`docker push [username_hub]/[image_name]:[tag]`

 - Ví dụ:
 
	`docker push longtran/apache_test:1.0`

 - Note: trong trường hợp image_name chưa có trên docker hub thì lúc bạn push lên, sẽ tự tạo ra một repository mới
 - Khi một người dùng muốn sử dụng image vừa được bạn tải lên, sẽ sử dụng lệnh pull:
 
	`docker pull longtran/apache_test:1.0`



