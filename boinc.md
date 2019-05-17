## Cài đặt boinc server trên máy ảo

Tải image Virtual Box máy ảo đã được cài sẵn BOINC server(mysql, apache và các daemon) tại: http://boinc.berkeley.edu/dl/debian-8-boinc-server-150927.7z

Máy ảo được tạo với user ```boincadm```, mật khẩu ```boincadmpw```, mật khẩu của ```root``` là ```rootpw```.

## Tạo project mới

cd tới boinc-src/tools

chạy lệnh ./make_project <OPTION> <tên_project>

copy file ~/projects/<tên_project>/<tên_project>.http.conf vào /etc/apache2/sites-availabe, chạy lệnh sudo a2ensite <tên_project>.http.conf để kích hoạt trang web của project.

## Biên dịch mã nguồn 

Chuyển mã nguồn vào thư mục ~/boinc-src/samples/example_app 

Sửa Makefile theo mẫu:

```
<tên_file>: <tên_file>.cpp libstdc++.a $(BOINC_API_DIR)/libboinc_api.a $(BOINC_LIB_DIR)/libboinc.a
	$(CXX) $(CXXFLAGS) $(CPPFLAGS) $(LDFLAGS) -o <tên_file> <tên_file>.o libstdc++.a -pthread \
	$(BOINC_API_DIR)/libboinc_api.a \
	$(BOINC_LIB_DIR)/libboinc.a
```

Chạy make.
