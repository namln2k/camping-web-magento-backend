# Setup môi trường:

## Hệ điều hành:
Khuyến nghị Ubuntu, đã test ở Ubuntu 20.04, Ubuntu 22.04

## Các service cần thiết:
- Docker: https://docs.docker.com/engine/install/ubuntu/
- Node: 
    - Khuyến nghị dùng Node.js version càng mới càng tốt, đã test với Node.js v21.5.0
    - Khuyến nghị cài bằng NodeSource PPA (Cài bằng apt trên Ubuntu thấy Node.js không lên được bản mới nhất)
    - Tutorial: https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-ubuntu-20-04#option-2-installing-node-js-with-apt-using-a-nodesource-ppa

# Setup backend
1. Clone project
```
git clone https://github.com/namln2k/camping-web-magento-backend.git backend && cd backend
```

2. Copy các folder tải từ drive vào project
```
cp -r path/to/folder/docker/ ./
cp path/to/file/db.sql ./
```

3. Khởi chạy các service
```
cd docker/environment && docker compose up -d
```
**Note**: Sửa biến WORK_DIR và VIRTUAL_HOST trong docker/environment/.env trước khi chạy (nếu cần)

4. Vào cli bash:
```
docker compose exec cli bash
```

5. Vào mysql với account root để tạo db và cấp quyền cho user magento
```
mysql -u root -h db -p
```
> mysql bash
```
CREATE DATABASE magento246p3;
GRANT ALL PRIVILEGES ON magento246p3.* TO 'magento'@'%' WITH GRANT OPTION;
```

6. Import database bằng user magento
```
mysql -u magento -h db -p magento246p3 < db.sql
```
**Note: Sửa base_url và base_secure_url trong core_config_data nếu cần


7. Cài các module cho project bằng composer và chạy deploy code ở chế độ developer
```
composer install && bin/magento setup:upgrade
```

8. Thử truy cập vào frontend và backend magento. Default là
- Frontend: http://magento246p3.local/
- Backend: http://magento246p3.local/admin
    - Username: ```admin```
    - Password: ```admin123```
- Kết quả thấy được như này là ok
    - Frontend: https://i.imgur.com/h1xBF2a.png
    - Backend: https://i.imgur.com/5lkpeq5.png

# Setup frontend:
1. Clone project
```
git clone https://github.com/namln2k/camping-web.git frontend && cd frontend
```

2. Copy các folder tải từ drive vào project
```
cp path/to/file/.env ./
```

3. Cài các package cho project bằng npm và chạy web lên ở chế độ dev
```
npm i && npm run dev
```