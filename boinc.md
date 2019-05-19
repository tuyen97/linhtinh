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

## Tạo application

1. cd tới thư mục project vừa tạo, ví dụ ~/project/hello

2. Sửa file ```config.xml``` theo mẫu:

```
<app>
	<name>cube</name>
	<user_friendly_name>OpenGL cube demo</user_friendly_name>
</app>
```

3. Chạy lệnh ./bin/xadd

## Tạo application version

Cấu trúc thư mục apps cần được tạo theo mẫu:

```
apps/
  appname1/
    1.0/
      windows_intelx86/
        (files)
      windows_intelx86__cuda/
        (files)
      i686-apple-darwin/
        (files)
      ... other platforms
    1.1/
    ... other versions
  appname2/
  ... other apps
```

Ví dụ với nền tảng x86_64-pc-linux-gnu, ta cần 1 file thực thi, 1 file version.xml, có thể thêm 1 file codesigning:

<b>version.xml0</b>:

```
<version>
	<file>
		<physical_name>hello</physical_name>
	</file>
</version>
```

Chạy lệnh ./bin/update_versions 

## Submit job

Sau khi thêm app version cho mỗi app, ta cần submit job dựa trên các app version này để BOINC lập lịch cho client.

Mỗi chương trình thường có file đầu vào và đầu ra, ta cần chỉ ra chúng cho client biết.

Đối với file đầu vào, trước khi sử dụng cần staged qua câu lệnh:

```bin/stage_file [--gzip] [--copy] [--verbose] path```

xem kết quả thực hiện lệnh này trong thư mục download của project

Sau đó, việc cấu hình được thông qua 2 file xml tương ứng cho input và output file, đặt trong thư mục templates, ví dụ:

<b>hello_world_in cho các input file</b>:

```<input_template>
        <workunit>
        </workunit>
</input_template>
```

<b>hello_world_out cho các ouput file</b>:

```
<output_template>
        <file_info>
        <name><OUTFILE_0/></name>
        <generated_locally/>
        <upload_when_present/>
        <max_nbytes>32768</max_nbytes>
        <url><UPLOAD_URL/></url>
    </file_info>
    <result>
        <file_ref>
            <file_name><OUTFILE_0/></file_name>
            <open_name>out.txt</open_name>
        </file_ref>
    </result>
</output_template>
```
Sau khi cấu hình xong các file đầu vào và ra cho app, chạy lệnh:

```./bin/create_work --appname infile_1 ... infile_n ```

với infile1..n là các file đã stage và được ghi trong input xml.

## Hoàn thành tạo project

Chạy lệnh ```./bin/start```

## Giao diện admin

Tại url_base/<tên_project>_ops


