# Docker Tutorial

**Docker là gì?**

- Docker là một nền tảng để đóng gói, triển khai và chạy ứng dụng trong các container.

- **Container** là một môi trường độc lập, chứa tất cả các thành phần cần thiết để chạy ứng dụng (mã nguồn, thư viện, công cụ, v.v.).


1. **Cài đặt Docker trên Kali Linux**

Mở terminal chạy: 

    sudo apt update
    sudo apt install docker.io -y

![img](https://github.com/DucThinh47/Docker-Tutorial/blob/main/images/image.png?raw=true)

Sau khi cài đặt xong, khởi động Docker:

    sudo systemctl start docker
    sudo systemctl enable docker

![img](https://github.com/DucThinh47/Docker-Tutorial/blob/main/images/image1.png?raw=true)

Kiểm tra xem Docker đã hoạt động chưa:

    sudo docker --version

![img](https://github.com/DucThinh47/Docker-Tutorial/blob/main/images/image2.png?raw=true)

=> Docker đã hoạt động.

2. **Chạy thử Container đơn giản**

Chạy container với hình ảnh mặc định của Ubuntu:

    sudo docker run -it ubuntu bash

Giải thích: 

- ***run***: Tạo và chạy container.

- ***-it***: Mở terminal tương tác.

- ***ubuntu***: Sử dụng image Ubuntu.

- ***bash***: Chạy shell bên trong container.

![img](https://github.com/DucThinh47/Docker-Tutorial/blob/main/images/image3.png?raw=true)

=> Chạy thành công container Ubuntu!

Thoát khỏi container bằng lệnh:

    exit

3. **Các lệnh cơ bản của Docker**

- Xem danh sách container đang chạy: 

        sudo docker ps

    Ví dụ, chạy container Ubuntu: 

    ![img](https://github.com/DucThinh47/Docker-Tutorial/blob/main/images/image4.png?raw=true)

    Xem danh sách container đang chạy: 

    ![img](https://github.com/DucThinh47/Docker-Tutorial/blob/main/images/image5.png?raw=true)

- Xem tất cả container (kể cả đã dừng):

        sudo docker ps -a

    ![img](https://github.com/DucThinh47/Docker-Tutorial/blob/main/images/image6.png?raw=true)

- Xóa container:

        sudo docker rm <container_id>
    
    ![img](https://github.com/DucThinh47/Docker-Tutorial/blob/main/images/image7.png?raw=true)

- Xóa image không sử dụng:

        sudo docker image prune -a
    
    ![img](https://github.com/DucThinh47/Docker-Tutorial/blob/main/images/image8.png?raw=true)

4. **Hiểu về Docker Images và Containers**

- ***Image***: Là một bản mẫu (template) chứa hệ điều hành và ứng dụng.

- ***Container***: Là một instance của image đang chạy.

- Các lệnh quan trọng: 

        sudo docker images              # Xem danh sách images
        sudo docker ps -a               # Xem danh sách containers
        sudo docker rm <container_id>   # Xóa container
        sudo docker rmi <image_id>      # Xóa image

5. **Làm việc với Dockerfile (Tạo Image tùy chỉnh)**

**Dockerfile** là gì? 

Dockerfile sử dụng DSL (Domain Specific Language) và chứa các hướng dẫn để tạo Docker image. Dockerfile sẽ xác định các quy trình để nhanh chóng tạo ra một image. Trong khi tạo ứng dụng, nên tạo một Dockerfile theo thứ tự vì Docker daemon chạy tất cả các hướng dẫn từ trên xuống dưới.

=> ***Dockerfile là source code của Dockerimage***

**Dockerfile commands**: 

- ***FROM***
    - Đại diện cho base image (Hệ điều hành), là lệnh được thực thi trước bất kỳ lệnh nào khác.

    - Cú pháp: *FROM < ImageName >*

    - Ví dụ: Hệ điều hành cơ sở sẽ là ubuntu:19.04:

            FROM ubuntu:19.04

- ***COPY***

    - Copy file hoặc thư mục vào image trong khi đang build.

    - Cú pháp: *COPY < Source >   < Destination >*

    - Ví dụ: copy file .war vào thư mục Tomcat webapps: 

            COPY target/java-web-app.war /usr/local/tomcat/webapps/java-web-app.war

- ***ADD***

    - Trong khi tạo image, có thể tải xuống các file từ các đích đến HTTP/HTTPs bằng lệnh ADD.

    - Cú pháp: *ADD < url >*

    - Ví dụ: download Jenkins:

            ADD https://get.jenkins.io/war/2.397/jenkins.war

- ***RUN***

    - Scripts và commands được chạy với lệnh RUN. Việc thực thi lệnh RUN hoặc hướng dẫn sẽ diễn ra trong khi tạo một image trên đầu các lớp trước (Image).

    - Cú pháp: *RUN < Command + ARGS>*

    - Ví dụ: 

            RUN touch file

- ***CMD***

    - Bắt đầu một tiến trình bên trong container và nó có thể bị ghi đè. 

    - Cú pháp: *CMD [command + args]*

    - Ví dụ: khởi động Jenkins: 

            CMD ["java", "-jar", "Jenkins.war"]

- ***ENTRYPOINT***

    - Một container sẽ hoạt động như được thực thi được cấu hình bởi ENTRYPOINT. Khi bắt đầu Docker container, một command hoặc script gọi là ENTRYPOINT được thực thi.

    - Không thể ghi đè, khác với CMD.

    - Cú pháp: *ENTRYPOINT [command + args]*

    - Ví dụ: thực thi câu lệnh echo

            ENTRYPOINT ["echo", "Welcome to HN"]

- ***MAINTAINER***

    - Xác định tác giả/ chủ sở hữu của Dockerfile, đồng thời cũng thiết lập tác giả/ chủ sở hữu của Dockerimage. 

    - Cú pháp: *MAINTAINER < NAME >*

    - Ví dụ: Thiết lập tác giả của image là GFG

            MAINTAINER  GFG author 


Để dựng lab trong Labtainer, cần tự tạo **Dockerfile**.

Ví dụ, tạo một image Ubuntu có sẵn ***net-tools***:

- Tạo file ***Dockerfile***:

    1. Tạo thư mục làm việc mới: 

        ![img](https://github.com/DucThinh47/Docker-Tutorial/blob/main/images/image9.png?raw=true)
    
    2. Tạo file ***Dockerfile***:

            nano Dockerfile
        
        ![img](https://github.com/DucThinh47/Docker-Tutorial/blob/main/images/image10.png?raw=true)
    
    3. Viết nội dung file: 

        ![img](https://github.com/DucThinh47/Docker-Tutorial/blob/main/images/image11.png?raw=true)
    
    4. Lưu và đóng file

- Xây dựng Docker image

    1. Xây dựng image từ Dockerfile

        - Trong thư mục chứa Dockerfile, chạy lệnh sau để xây dựng image:

                sudo docker build -t my_ubuntu .
            
            - ***-t my_ubuntu***: Đặt tên cho image là my_ubuntu.

            - ***.***: Chỉ định thư mục hiện tại (nơi chứa Dockerfile).

            ![img](https://github.com/DucThinh47/Docker-Tutorial/blob/main/images/image12.png?raw=true)

        - Docker sẽ thực hiện các bước trong Dockerfile:

            - Tải image ubuntu:latest từ Docker Hub.

            - Cập nhật hệ thống và cài đặt net-tools.

            - Tạo image mới với tên my_ubuntu.

        - Sau khi xây dựng xong, có thể kiểm tra image vừa tạo bằng lệnh:

                sudo docker images
            
            ![img](https://github.com/DucThinh47/Docker-Tutorial/blob/main/images/image13.png?raw=true)

- Chạy container từ image

    - Chạy container từ image my_ubuntu bằng lệnh sau:

            sudo docker run -it my_ubuntu

        - ***-it***: Khởi động container ở chế độ tương tác (interactive terminal).

        - ***my_ubuntu***: Tên image vừa tạo.
    
    - Khi container khởi động, sẽ thấy dòng lệnh của Ubuntu (vì CMD ["bash"] trong Dockerfile đã thiết lập lệnh mặc định là bash).

    ![img](https://github.com/DucThinh47/Docker-Tutorial/blob/main/images/image14.png?raw=true)

6. **Docker Volumes (Lưu dữ liệu giữa các container)**

Khi làm lab, có thể sẽ cần lưu file cấu hình hoặc dữ liệu giữa các lần chạy container.

    sudo docker volume create my_data
    sudo docker run -it -v my_data:/data ubuntu bash

Sau đó, dữ liệu trong /data sẽ không bị mất khi container bị xóa.

7. **Docker Networking (Giao tiếp giữa các container)**

Lab thường sẽ cần nhiều container giao tiếp với nhau. Docker có thể giúp tạo mạng nội bộ giữa các container.

    sudo docker network create my_network
    sudo docker run -it --network=my_network --name=ubuntu1 ubuntu bash
    sudo docker run -it --network=my_network --name=ubuntu2 ubuntu bash

Bây giờ, ubuntu1 và ubuntu2 có thể ping nhau bằng tên container.











    





