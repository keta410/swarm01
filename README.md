# swarm01

**MY URL**=> https://spcn22wp-mysql.xops.ipv9.me

**Wakatime Project: swarm01**=> https://wakatime.com/@spcn22/projects/gtauhbzafe

**Ref** :
- App [wordpress-mysql] , I want deploy on portainer

  App [wordpress-mysql] ที่ต้องการ deploy บน portainer
>(https://github.com/docker/awesome-compose/tree/master/wordpress-mysql)


* The Example for code about traefik

    ตัวอย่าง code เกี่ยวกับ traefik
>(https://github.com/pitimon/dockerswarm-inhoure/blob/main/ep04-hello-world-revProxy/hello-world-https.yml) 

>(https://doc.traefik.io/traefik/user-guides/docker-compose/acme-http/)

______________________________________________________

Contents swarm01 (สารบัญเนื้อหา swarm01)
-----------------
No. |English Title (ชื่อเรื่องภาษาอังกฤษ)  | Thai Title (ชื่อเรื่องภาษาไทย) |
----- |----- | ----- |
1)|[Prepare Device ((personal computer, notebook)](https://github.com/keta410/swarm01#prepare-device-personal-computer-notebook)|  เตรียมพร้อมอุปกรณ์ (คอมพิวเตอร์, โน๊ตบุ๊คส่วนตัว)| 
2)|[Step 1 Create Folder and File for APP](https://github.com/keta410/swarm01#step-1-create-folder-and-file-for-app)|ขั้นตอนที่ 1 สร้าง Folder และ File สำหรับ APP|
3)|[Step 2 Deploy on Portainer(.xops.ipv9.me)](https://github.com/keta410/swarm01#step-2-deploy-on-portainerxopsipv9me)|ขั้นตอนที่ 2 การ Deploy ขึ้นบน Portainer(.xops.ipv9.me)|
4)|[Expected Result](https://github.com/keta410/swarm01#expected-result)|ผลลัพธ์ที่คาดว่าจะได้รับ|
5)|[Working Principle in docker-compose](https://github.com/keta410/swarm01#working-principle-in-docker-compose)|หลักการทำงานใน docker-compose|
______________________________________________________
## **Prepare Device** (personal computer, notebook)
เตรียมพร้อมอุปกรณ์ (คอมพิวเตอร์, โน๊ตบุ๊คส่วนตัว)
______________________________________________________
1. Create **VM** on proxmox then set timezone etc.

    สร้าง **VM** บน proxmox จากนั้นตั้งค่า timezone เป็นต้น

    <ins>Form command</ins> :

    ```linux
    timedatectl set-timezone Asia/Bangkok   
    ```

2. Check the **IP address** of **VM** created (I created a VM named : keta-Docker-115) and copy the IP address.

    ตรวจสอบ **IP address** จาก VM ที่สร้าง (ในที่นี้สร้าง **VM** ชื่อ : keta-Docker-115)

    <ins>Form command</ins> :
    
    ```linux
    ip a
    ```
3. Click in the bottom-left cornor of the window named [**Open Remote Window**] and this will appear

    คลิกปุ่มด้านล่างซ้ายสุดของหน้าต่าง ชื่อว่า [**Open Remote Window**] และจะปรากฏสิ่งนี้

    ![config-host1](https://user-images.githubusercontent.com/104758471/222904598-4065cc30-cc6e-457d-84c5-b3bd852f0bfc.jpg)

4. Click [**Connect to Host...**] > [**Configure SSH Hosts..**] > [**C:\Users\asus\.ssh\config**] then configure as follows 

    คลิก [**Connect to Host...**] > [**Configure SSH Hosts..**] > [**C:\Users\asus\.ssh\config**] จากนั้นกำหนดค่าตามนี้

<ins>Form config</ins>:

    ```
    Host (name's host)          //you set on personal computer
                                //ชื่อ host ที่ตั้งบนเครื่องคอมพิวเตอร์
        HostName (ip address)   //config ip address from proxmox
                                //กำหนดค่า ip address จาก proxmox
        User (account to Login on proxmox)  //config account name from proxmox
                                            //กำหนดชื่อบัญชีจาก proxmox
    
    ```
![host-config](https://user-images.githubusercontent.com/104758471/222712916-22567d26-bcb9-4060-9bd8-410b1b0f3192.jpg)

5. Install **Docker, wakatime** and **ssh remote** at the computer used on VS Code then remote own VM from proxmox with the IP address, I coppied.
    
    ติดตั้ง **Docker, wakatime** และ **ssh remote** ที่เครื่องคอมพิวเตอร์ที่ใช้งานผ่าน VS Code จากนั้น remote VM ของตัวเองจาก proxmox ด้วย IP adress ที่ copy มา

    * Docker engine install on ubutu
        การติตดั้ง Docker engine บน ubutu

     <ins>Form command</ins> :
    ```linux
    apt update ; apt upgrade -y
    apt-get install \
    ca-certificates \
    curl wget \
    gnupg \
    lsb-release -y
    ```
    ```linux
    mkdir -m 0755 -p /etc/apt/keyrings
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg
    ```
    ```linux
    echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) stable" |  tee /etc/apt/sources.list.d/docker.list > /dev/null
    ```
    ```linux
    apt-get update
    apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
    ```    

    When finished in the VM, right click to [**Convert to template**] and clone it out.

    เมื่อลงใน VM เสร็จแล้ว ให้ทำเป็น Template โดยคลิกขวาเลือก [**Convert to template**] แล้ว clone ออกมา

6. Set work specification that has already been defined, All three clones were named : ket-manager-190, ket-worker1-191, ket-worker2-192

    กำหนดสเปคการทำงานที่กำหนดไว้ จากนั้นตั้งชื่อตัวที่ clone ทั้งหมด 3 เครื่อง ได้แก่ ket-manager-190, ket-worker1-191, ket-worker2-192

7. In No.6, All clones, change hostname, set timezone and remove machine-id from master machine. Install wakatime extension and ssh remote on all clone machine 

    จากข้อ 6. ตัวที่ clone ทั้งหมด ให้ทำการเปลี่ยนชื่อ hostname, ตั้งค่า timezone และลบ machine-id จากเครื่องที่เป็น master ออก จากนั้นติดตั้ง Extensions ของ wakatime และ ssh remote ลงบนเครื่องที่ clone ทุกตัว 

* Change *hostname* and *timezone*
    
    เปลี่ยนชื่อ *hostname* และ *timezone* 
    ```linux
    sudo -i
    hostnamectl set-hostname [set name]
    timedatectl set-timezone Asia/Bangkok
    ```
* Edit *machine-id* in clone
    
    แก้ไข *machine-id* ในตัว clone 
    ```linux
    cp /dev/null /etc/machine-id
    rm /var/lib/dbus/machine-id
    ln -s /etc/machine-id /var/lib/dbus/machine-id
    init 0
    ```

8. Assign Docker permissions to the user on the clone machine

    กำหนดสิทธิ์การใช้งาน Docker ให้กับ user บนเครื่องที่ clone ออกมา
```shell
sudo usermod -aG docker $USER       //USER บัญชีที่เราไว้ login บนเครื่อง 
docker ps                           //เป็นการเช็คว่า user สามารถเข้าใช้งาน docker ได้
```

9. Create **Docker Swarm** on the manager machine using command ```docker swarm init``` and it will show a token, copy the manager machine token and run it on 2 worker machines

    สร้าง **Docker Swarm** บนเครื่องชื่อ manager โดยใช้คำสั่ง ```docker swarm init``` แล้วมันจะแสดง token ให้ copy token ของเครื่อง manager ไป run บนเครื่องที่เป็น worker 2 เครื่องที่เหลือ

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

<details><summary>CLICK SHOW CODE (docker-compose.yml)</summary>
<p>

```docker 
version: '3.8'      #Version Docker
services:
    db:         #Data
        image: mariadb:10.6.4-focal     
        #Mariadb version used (version ของ mariadb ที่ใช้)
        command: '--default-authentication-plugin=mysql_native_password'
        volumes:
            - db_data:/vat/lib/mysql    
            #Set path to collect data in db_data (ตั้งค่าเส้นทางในการเก็บข้อมูลมาใน db_data)
        restart: always
        networks:       #Network, db used (Network ที่ db ใช้)
            - wb
                        
        environment:    
        #Environment of mariadb (ทรัพยากรของ mariadb)
            - MYSQL_ROOT_PASSWORD=somewordpress
            - MYSQL_(config..)=wordpress
            .
            .
        #Config about this => DATABASE,USER,PASSWORD (กำหนดเกี่ยวกับ => DATABASE,USER,PASSWORD)
            
        expose:
            - 3306
            - 33060

    wordpress:      #App
        image: wordpress:latest     
        #Wordpress version used (version ของ wordpress ที่ใช้)
        restart: always 
        networks:       #Network, wordpress used (Network ที่ wordpress ใช้)
            - wp
            - webproxy

        environment:    
        #Environment of wordpress (ทรัพยากรของ wordpress)
            - WORDPRESS_DB_HOST=db
            - WORDPRESS_DB(config..)=wordpress
            .
            .
            #Config about them => USER,PASSWORD,NAME (กำหนดเกี่ยวกับ => USER,PASSWORD,NAME)
            
        deploy:    
            replicas: 1     #Config num of device to deploy (กำหนดจำนวนของเครื่องที่จะ deploy)
                            
            labels:     #Set label for app by connecting to Traefik (กำหนด label สำหรับ app โดยเชื่อมต่อกับ Traefik)
                        
              - traefik.docker.network=webproxy     
            #Name network of Traefik = webproxy (ชื่อ netwrok ของ Traefik = webproxy)
              - traefik.enable=true                 
            #Set status of Traefik (ตั้งค่าสถานะของ Traefik)
              - traefik.constraint-label=webproxy   
            #Define the name of Traefik to run(กำหนดชื่อ treafik ที่ต้องการให้ทำงาน)
              - traefik.http.routers.${APPNAME}-https.entrypoints=websecure         
            #Set port to connect with a request sent (กำหนด port ในการเชื่อมต่อมีคำขอส่งมา)            
              - traefik.http.routers.${APPNAME}-https.rule=Host("${APPNAME}.xops.ipv9.me")  
            #Set name domain for con(ตั้งค่าชื่อ domain การเข้าถึง)
              - traefik.http.routers.${APPNAME}-https.tls.certresolver=default      
            #Create URL certificate (สร้างใบรับรอง URL) 
              - traefik.http.services.${APPNAME}.loadbalancer.server.port=80    
            #Set port in the request(ตั้งค่า port ในการร้องขอ)
              - traefik.http.routers.${APPNAME}-https.tls=true                
            #Set TSL to work (เปิดใช้งาน TSL)     

        #It's variable (เป็นตัวแปร) 
        #Config APPNAME=spcn22wp-mysql (กำหนดค่า APPNAME=spcn22wp-mysql)

volumes:  
    db_data:

networks:       #All network, can used
                #network ทั้งหมดที่สามารถใช้ได้
    
    wb:                 #network that allows communication between db and wordpress
                        #network ที่สร้างให้สามารถข้อมูลสื่อสารระหว่าง db กับ wordpress ได้
      driver: overlay

    webproxy:              
      external: true        #config this app can used webproxy network 
                            #กำหนดให้แอพนี้สามารถใช้ webproxy network ได้
```

</p>
</details>
______________________________________________________

### **Step 2** Deploy on Portainer(.xops.ipv9.me)
ขั้นตอนที่ 2 การ Deploy ขึ้นบน Portainer(.xops.ipv9.me)
______________________________________________________

1. Open website =>(https://portainer.ipv9.me/#!/auth) and Login with GitHub account

    เปิด website =>(https://portainer.ipv9.me/#!/auth) และ Login with GitHub account

2. Press [**Add Stack**] and click [**Web editor**]

    กดปุ่ม  [**Add Stack**] และคลิก [**Web editor**]

* Add Stack
![up-portainer-fillText jpg ](https://user-images.githubusercontent.com/104758471/222903882-8aa8a92d-63bc-4dc4-8615-0070078cd863.jpg)

* Web Editor
![up-portainer-wp-mysql-fillText jpg ](https://user-images.githubusercontent.com/104758471/222903804-43174bd9-3608-4865-85d4-1f2121bf627a.jpg)
3. Copy the code in the file docker-compose.yml and press [**Deploy the stack**] 
    
    Copy code ในไฟล์ docker-compose.yml และกด [**Deploy the stack**]
    
     
______________________________________________________

### **Expected Result**
ผลลัพธ์ที่คาดว่าจะได้รับ
______________________________________________________

* After add stack

    หลัง add stack

![up-portainer-wp-mysql-complete jpg ](https://user-images.githubusercontent.com/104758471/222713931-b48cef9c-fbf7-455c-97ae-a168718d0f17.jpg)

* Web browsers, I configure can access WordPress

    เว็บเบราว์เซอร์ที่เรากำหนดให้สามารถเข้าถึง WordPress ได้

![access-web-wp-mysql](https://user-images.githubusercontent.com/104758471/222714145-31addf7d-4858-495d-934a-212e289700e1.jpg)

______________________________________________________

### **Working Principle in docker-compose**
หลักการทำงานใน docker-compose
______________________________________________________
    
<ins>Under the ```docker-compose.yml``` of the app [**wordpress-mysql**] is specified</ins> :

ภายใต้การทำงานของ ```docker-compose.yml``` ของ app [**wordpress-mysql**] มีการระบุ

- Version of the compose file directly, it can specify any version that is supported by the app I choose, in this case I choose 3.8

    version ของ compose file โดยตรงนี้ เราสามารถระบุเป็น version ที่เท่าไหร่ก็ได้ที่สนับสนุนกับ app ที่เราเลือก ในที่นี้เลือก 3.8

- Service spedifies the container to use. There are **db** and **wordpress**. In the container, 
consists of image, command, volumes, restart, networks, environment, expose and deploy etc. None of these require all containers, because it depends on what we want to do.

    service ใช้ระบุ container ที่จะใช้ ในที่นี้มี db และ wordpress โดยภายใน container เรานี้ประกอบไปด้วย image, command, volumes, restart, networks, environment, expose และ deploy เป็นต้น ทั้งหมดที่กล่าวมาในทุก container ไม่จำเป็นต้องใช้ทั้งหมดเพราะขึ้นอยู่กับวัตถุประสงค์ว่าเราต้องการทำอะไรบ้าง 

- Volumes are used to create storage. This is different from the volumes in the service because that section is linked to the individual volumes of the created container.

    volumes ใช้สร้างที่เก็บข้อมูล ซึ่งแตกต่างจาก volumes ที่อยู่ใน service เพราะตรงในส่วนนั้นจะเป็นการเชื่อม volumes แต่ละตัว container ที่สร้างไว้ 

- Networks are connections of existing or self-contained, networks communicating with compose files.

    networks เป็นการเชื่อมต่อกันของ network ที่มีหรือสร้างเอง สื่อสารกับ compose file

<ins>Summary</ins>

When command ```docker compose up -d``` or Click right the ```docker-compose.yml``` file, then select [**Compose Up**]. this will cause the container to be created, the network will record the data specified for each on the container, the port that is specified or a domain name that uses traefik to help manage as we define.

<ins>สรุป</ins>

เมื่อสั่ง ```docker compose up -d``` หรือคลิกขวาที่ไฟล์ ```docker-compose.yml``` เลือก [**Compose Up**] ขึ้นไปจะทำให้มีการสร้างตัว container ,network มีการบันทึกข้อมูลตามที่กำหนดไว้ในแต่ละตัวบน container, port ที่ถูกระบุใช้งาน หรือชื่อโดเมนที่มีการใช้ traefik ช่วยในการจัดการตามที่เรากำหนด