# ����ͬ������CanalClientEx
------
  ����Ӧ�ó�����

> * ��������������ݿ�ʵʱ���ݣ�
> * ���ݿ��ʵʱ�䶯�����֪ͨ������֪ͨˢ�»��棬��ṹ�޸ķ��͵��ʼ���ȣ���
> * ���ݿ����ȫһ����ʵʱͬ���������趨���ֶ�ӳ�䣬������һЩ�����ֶΣ�����ƴ���룬��ƴ���򵥼���ӳ��ȣ���

  ����Ŀ����**canal 1.0.24**���°�������޸ģ���չ�˷���˺Ϳͻ��˵�һЩ���ܣ���Ҫ������
> * �ͻ��ˣ����ݿ⾵�񱸷ݣ����������������ͬ�����⣩��
> * �ͻ��ˣ����ݿ���ӳ��ͬ�������Զ�����ƴ���룬��ƴ���ֶμ���ȣ���
> * �ͻ��ˣ����ݿ�ָ��������ݱ䶯֪ͨ��Java�ࣻ
> * �ͻ��ˣ�֧�����ݿ��MySQL->MS SQL��ͬ����
> * ����ˣ�positionλ��ͬ����Redis��Ⱥ���Ա���File����µ�������Ӳ�̻�����������޷���ͬ����
> * ����ˣ�ͬ���쳣��Ԥ���ʼ���
> * ����ˣ��޸�bat�ļ��޷���win10���е����⣻

[�ͻ���1.0.24����](https://github.com/kongshanxuelin/canalClientEx/files/1375074/canal.canalClientEx-1.0.24.zip)   [�����1.0.24����](https://github.com/kongshanxuelin/canalClientEx/files/1375087/canal.deployer-1.0.24.tar.gz)���������QQȺ���ۣ�[![QQ](http://pub.idqqimg.com/wpa/images/group.png)](https://jq.qq.com/?_wv=1027&k=5onpjJC)

## ��ʼʹ��

* ȷ��MySQL5.5+����������binlog��
* ����canal.deployer-1.0.24.tar.gz(����ˣ�,canal.canalClientEx-1.0.24.tar.gz(�ͻ��ˣ�
* ��ѹ�����ã���������ͬ��ʵ������confĿ¼����Ӷ���ļ��м��ɣ�������ÿ��Ŀ¼�µĵ�instance.properties����exampleĿ¼������canal.instance.mysql.slaveId(�����ظ��������ݿ���Ϣ��canal.instance.master.address��canal.instance.dbUsername��canal.instance.dbPassword��
* ����
    * Windows������ˣ�����bin\startup.bat���ɣ����ͻ��ˣ�����bin\startup.bat���ɣ�
    * Linux��ȷ��binĿ¼�µ�sh�ļ���ִ��Ȩ�ޣ�����ˣ�����bin\startup.sh���ɣ����ͻ��ˣ�����bin\startup.sh���ɣ�

## �������ǿ

* ���룺
mvn clean install -Dmaven.test.skip -Denv=release

* ��canal.properties�м����������ԻὫposition��Ϣͬ����Redis��Ⱥ��
redis.server=192.168.1.170:7000,192.168.1.170:7001,192.168.1.170:7002,192.168.1.215:7000,192.168.1.215:7001,192.168.1.215:7002

* ��������ʱ���ʼ�Ԥ��

## �ͻ�����ǿ
* ֧�����ݿ⾵�񱸷�,config.xml�нڵ��������Ϣ���£�
```xml
    <node name="test" desc="test mirror db">
        <canal-server-mode>simple</canal-server-mode>  
		<canal-server-ip>127.0.0.1</canal-server-ip>
		<canal-server-port>PORT</canal-server-port>
    	<canal-server-inst>INSTNAME</canal-server-inst>
		<!-- �Ƿ���Ч -->
		<active>true</active>
		<!-- ͬ�������JDBC���� -->
		<db-url><![CDATA[jdbc:mysql://IP:PORT/test?useUnicode=true&characterEncoding=UTF-8]]></db-url>
		<db-driver>com.mysql.jdbc.Driver</db-driver>
		<db-username>USER</db-username>
		<db-password>PASSWORD</db-password>
		<db-schema>test</db-schema>
	</node>
```
* ֧�ֱ����ݺϲ�ͬ����֧���ֶ�ӳ�䶨�壬֧��ͬ��Ԥ���ȣ�
```xml
	<node name="test2" desc="test table mapping">
			<canal-server-mode>simple</canal-server-mode>  
			<canal-server-ip>127.0.0.1</canal-server-ip>
			<canal-server-port>PORT</canal-server-port>
    	    <canal-server-inst>INSTNAME</canal-server-inst>
			<!-- �Ƿ���Ч -->
			<active>true</active>
			<!-- ͬ�������JDBC���� -->
			<db-url><![CDATA[jdbc:mysql://IP:PORT/test?useUnicode=true&characterEncoding=UTF-8]]></db-url>
			<db-driver>com.mysql.jdbc.Driver</db-driver>
			<db-username>USER</db-username>
			<db-password>PASSWORD</db-password>
			<tables>
				<table source-scheme-name="test" source-name="test" dest-name="test_dest" ddlSync='true' rule="AND" dest-name-pri="df" >
					<fields>
						<field name="df" text="df" />
						<field name="dff" type="py" text="dff"></field>
						<field name="c2" type="el" text="df*2"/>
					</fields>
				</table>
			</tables>
			<alarms>
				<alarm stype="ddl" type="email">
					<title>�ʼ�����#{tableName}����ֶη����˱仯</title>
					<body>#{tableName}����ֶη����˱仯,ִ�е�SQL:#{sql}</body>
					<sendTo>xxx@xxx.com</sendTo>
				</alarm>
				<alarm stype="exception" type="email">
					<title>ͬ������#{source-scheme-name}.#{source-name}�����쳣����</title>
					<body>#{body}</body>
					<sendTo>xxx@xxx.com</sendTo>
				</alarm>
			</alarms>
	</node>
```
* ֧���Զ������ͬ������
```xml
	<node name="test3" desc="sync_gjk_eform">
		<canal-server-mode>simple</canal-server-mode>  
		<canal-server-ip>IP</canal-server-ip>
		<canal-server-port>PORT</canal-server-port>
		<canal-server-inst>INSTNAME</canal-server-inst>
		<active>true</active>
		<db-url>jdbc:mysql://IP:PORT/db_cdb?useUnicode=true&amp;characterEncoding=UTF-8</db-url>
		<db-driver>com.mysql.jdbc.Driver</db-driver>
		<db-username>USER</db-username>
		<db-password>PASSWORD</db-password>
		<db-schema>xxxx</db-schema>
		<db-trigger>com.xxx.yyy.TCustomerTableTrigger</db-trigger>
	</node>
```