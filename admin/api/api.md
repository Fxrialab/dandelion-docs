# Các api của dandelion admin

## API admin: 

- login: Đăng nhập
- Register: Đăng ký thành viên quản lý
- Profile: Thông tin ptofile của mỗi thành viên khi đăng nhập thành công
- adminactive: cho admin này được quản lý hay không
- forgotPassword:  thay đổi mật khẩu

## API user:

- users: list tất cả các thành viên đã đăng ký
- user: hiện thị thông tin chi tiết của mỗi thành viên gồm có thông tin thành viên, số lần post bài, số lần like, comment...
- useractive: cập nhật status của user, nếu status mà bằng 1 thì hoạt động, ngược lại là không.

## API posts:

- posts: hiện thị tất cả các bài viết của các thành viên.
- postactive: cho bài biết này được active hay không

## API comments:

- comments: hiện thị tất cả các bình luận của các thành viên.
- commentactive: cho bài bình luận này được active hay không

## API groups: 

- groups: hiện thị tất cả các nhóm của các thành viên

## API photos:

- photos: hiện thị tất cả các hình ảnh của các thành viên đã đăng.

## API themes:

- themes:  hiện thị tất cả các loại theme
- install: Cài đặt theme cho web
- remove: xóa theme
- detail: chi tiết themes 

## API plugin:

- plugin: hiện thị tất cả các plugin
- detail: chi tiết plugin
- install: cài đặt plugin
- remove: xóa plugin
- upload: upload các plugin

-
