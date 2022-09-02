# Linux Service등록

Service로 등록하여 systemd에서 관리를 할 수 있음.

참조사이트 : [CentOS7 서비스(service) 등록](https://lifeplan-b.tistory.com/96)

서비스파일을 경로

/usr/lib/systemd/system/서비스이름.service

or

/etc/systemd/system/서비스이름.service



[Unit]
Description=이름
After=network.target
After=syslog.target

#After=network.target syslog.target (띄어쓰기로 써도무방)

[Service]
Type=forking
Environment="PATH=PATH경로"
User=root
Group=root
ExecStart="실행경로"
ExecStop="스탑경로"
SuccessExitStatus=143

#자바의 경우 systemctl stop 서비스이름이 되었을 시

#143으로 보내줘야 종료를 한다고 함.

#참조사이트 : [linux - unit falling into a failed state (status=143) when stopping service - Stack Overflow](https://stackoverflow.com/questions/45953678/unit-falling-into-a-failed-state-status-143-when-stopping-service)

[Install]
WantedBy=multi-user.target
