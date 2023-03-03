# swarm01

## **Prepare Device** (personal computer, notebook)
เตรียมพร้อมอุปกรณ์ (คอมพิวเตอร์, โน๊ตบุ๊คส่วนตัว)
______________________________________________________
1. Create **VM** on proxmox, then set timezone etc.

    สร้าง **VM** บน proxmox จากนั้นตั้งค่า timezone เป็นต้น
2. Check the **IP address** of **VM** created (I created a VM named:keta-Docker-115) and copy the IP address.

    ตรวจสอบ **IP address** จาก VM ที่สร้าง (ในที่นี้สร้าง **VM** ชื่อ:keta-Docker-115)

    <ins>Form command</ins> :
    ```
    ip a
    ```

3. Install **Docker, wakatime** and **ssh remote** at the computer used on VS Code then remote own VM from proxmox with the IP address, I coppied.
    
    ติดตั้ง **Docker, wakatime** และ **ssh remote** ที่เครื่องคอมพิวเตอร์ที่ใช้งานผ่าน VS Code จากนั้น remote VM ของตัวเองจาก proxmox ด้วย IP adress ที่ copy มา

4. Click in the bottom-left cornor of the window named [Open Remote Window] and this will appear

คลิกปุ่มด้านล่างซ้ายสุดของหน้าต่าง ชื่อว่า [Open Remote Window] และจะปรากฏสิ่งนี้

![vs-code-remote2](https://user-images.githubusercontent.com/104758471/222713225-aa994fe2-8283-476e-b9bc-52a26e0977db.jpg)

5. Click [Connect to Host...] > [Configure SSH Hosts..] > [C:\Users\asus\.ssh\config] then configure as follows 

คลิก [Connect to Host...] > [Configure SSH Hosts..] > [C:\Users\asus\.ssh\config] จากนั้นกำหนดค่าตามนี้

    Form config:
    ```
    Host (name's host)          //you set on personal computer
                                //ชื่อ host ที่ตั้งบนเครื่องคอมพิวเตอร์
        HostName (ip address)   //config ip address from proxmox
                                //กำหนดค่า ip address จาก proxmox
        User (account to Login on proxmox)  //config account name from proxmox
                                            //กำหนดชื่อบัญชีจาก proxmox
    
    ```
![host-config](https://user-images.githubusercontent.com/104758471/222712916-22567d26-bcb9-4060-9bd8-410b1b0f3192.jpg)

<ins>Descriptrion</ins>:

คำอธิบาย :

\-> VM : Virtual Machines

\-> VS Code : Visual Studio Code

______________________________________________________

### **Step 1** Create Folder and File for APP
ขั้นตอนที่ 1 สร้าง Folder และ File สำหรับ APP
______________________________________________________

1. Create Folder Name's => **wordpress-mysql**
    
    สร้าง Folder ชื่อ => **wordpress-mysql**
2. Create File in folder => *wordpress-mysql*

    สร้าง File ใน folder ชื่อ => *wordpress-mysql* 

    <ins>Stucture of *wordpress-mysql* folder</ins> :
    
    โครงสร้างของ folder *wordpress-mysql* : 
    ```
    .
    |__ docker-compose.yml

    ```

3. Code for APP
    
   Code สำหรับ APP  

    3.1 ```docker-compose.yml```  
```docker 
version: '3.8'
services:
    db:
        image: mariadb:10.6.4-focal
        command: '--default-authentication-plugin=mysql_native_password'
        volumes:
            - db_data:/vat/lib/mysql
        restart: always
        networks:
            - wb
        environment:
            - MYSQL_ROOT_PASSWORD=somewordpress
            - MYSQL_(config..)=wordpress
            .
            .
            #Config about this => DATABASE,USER,PASSWORD

        expose:
            - 3306
            - 33060
    wordpress:
        image: wordpress:latest
        restart: always
        networks:
            - wp
            - webproxy
        environment:
            - WORDPRESS_DB_HOST=db
            - WORDPRESS_DB(config..)=wordpress
            .
            .
            #Config about them => USER,PASSWORD,NAME
            #กำหนดเกี่ยวกับ => USER,PASSWORD,NAME
        deploy:
            replicas: 1
            labels:
              - traefik.docker.network=webproxy
              - traefik.enable=true
              - traefik.constraint-label=webproxy
              - traefik.http.routers.${APPNAME}-https.entrypoints=websecure
              - traefik.http.routers.${APPNAME}-https.rule=Host("${APPNAME}.xops.ipv9.me")
              - traefik.http.routers.${APPNAME}-https.tls.certresolver=default
              - traefik.http.services.${APPNAME}.loadbalancer.server.port=80
              - traefik.http.routers.${APPNAME}-https.tls=true
        #It's variable 
        #Config APPNAME=spcn22wp-mysql

        #เป็นตัวแปร 
        #กำหนดค่า APPNAME=spcn22wp-mysql
volumes:
    db_data:
networks:
    wb:
      driver: overlay
    webproxy:
      external: true
```
______________________________________________________

### **Step 2** Deploy on Portainer(.xops.ipv9.me)
ขั้นตอนที่ 2 การ Deploy ขึ้นบน Portainer(.xops.ipv9.me)
______________________________________________________

1. Open website =>(https://portainer.ipv9.me/#!/auth) and Login with GitHub account
2. Press [**Add Stack**] and click [**Web editor**]

![up-portainer-fillText jpg ](https://user-images.githubusercontent.com/104758471/222713507-27708398-c60e-414e-a0a0-feb0112d8d49.jpg)
![up-portainer-wp-mysql-fillText jpg ](https://user-images.githubusercontent.com/104758471/222713725-2977dd7e-1f47-4b61-88d5-1432a09077aa.jpg)

3. Copy the code in the file docker-compose.yml and press [**Deploy the stack**] 

______________________________________________________

### **Expected Result**
ผลลัพธ์ที่คาดว่าจะได้รับ
______________________________________________________

After add stack

หลัง add stack

![up-portainer-wp-mysql-complete jpg ](https://user-images.githubusercontent.com/104758471/222713931-b48cef9c-fbf7-455c-97ae-a168718d0f17.jpg)

Web browsers, I configure can access WordPress

เว็บเบราว์เซอร์ที่เรากำหนดให้สามารถเข้าถึง WordPress ได้
URL:
>(spcn22wp-mysql.xops.ipv9.me)

![access-web-wp-mysql](https://user-images.githubusercontent.com/104758471/222714145-31addf7d-4858-495d-934a-212e289700e1.jpg)

Ref : 

* App[wordpress-mysql] , I want deploy on portainer

App[wordpress-mysql] ที่ต้องการ deplou บน portainer
>(https://github.com/docker/awesome-compose/tree/master/wordpress-mysql)


* The Example for code about traefik

ตัวอย่าง code เกี่ยวกับ traefik
>(https://github.com/pitimon/dockerswarm-inhoure/blob/main/ep04-hello-world-revProxy/hello-world-https.yml) 
