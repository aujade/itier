 Version 1.0

�˳����� node 0.6.6 ~ 0.7.4 �²���ͨ��

����԰�������������������Գ���:

####1 ���� ```sample``` Ŀ¼�µ� ```service.js```

   ```
      node service.js &
   ```

   ������̻���3561,3562 �˿��ṩhttp ����,��ģ����ʵ������Դ
   ��curl �������£�

   ```
      curl http://localhost:3561/?fm=m
   ```

   response :

   ```
      { "columns" : ["id","sex"],
        "data" : [ 
          [1,"m"],
          [2,"m"],
          [5,"m"],
          [7,"m"],
          [9,"m"]
        ]
      }
   ```

   ```
      curl http://localhost:3562/?minId=4
   ```

   response:

   ```
      { "columns" : ["id","name","type"], 
        "data" : [ 
          [5,"n5","t5"],
          [6,"n6","t6"],
          [7,"n7","t7"],
          [8,"n8","t8"],
          [9,"n9","t9"],
          [10,"n10","t10"]
        ]
      }
   ````

####2 ȷ��loader ������ȷ
   �� ```src/source/``` �£�```service_a.js``` ,```service_b.js``` Ϊ���������˿ڷ�����Ӧ��loader��
   ���Ĭ�ϵ�3561,3562 �˿ڱ�ռ�ã���ȷ������������Ӧ�޸�

####3 ȷ��sql ģ������
   ĿǰiTier ��֧������Ŀ¼���ã��鿴 ```resources/test/sources/test.json```,
   ���� ������ ```test_service_a``` ,```test_service_b``` ��Ӧ��sqlģ��:

   ```
   "test_service_a" : {
     "config":"select id as num from service_a.table where ##fm## order by num DESC limit 1,2",
     "params":{
       "fm":[{
         "type":"string",
         "pos":"##fm##"
       }]
     }
   }
   ```


####4 ���� iTier ���� 
   ���� ```run.sh``` ,Ĭ�ϻ��� 3459 �˿ڽ��м��������ü� ```conf.js```��

####5 ���Է��� 
   ��������Բ�����sqlģ�����õ����з���
   ���Է���```test_service_a```:
   �������

   ```
	curl http://127.0.0.1:3459/test/sources/test/test_service_a/where/fm:eq:f
   ```

   ��̨��������̨��� ```test_service_a``` ��ģ������ ``` ##fm## ``` �滻Ϊ ```fm=m```
   ���ؽ����

   ```
      { "columns" : ["num"],
        "data":[
          [8],
          [6]
        ]
      }
   ```
  
   ���Է���```test_servie_b```

   �������

   ```
	curl http://127.0.0.1:3459/test/sources/test/test_service_b/where/minId:eq:3
   ```

   ���ؽ����

   ```
      { "columns":["name"],
        "data":[
          ["n9"],
          ["n8"],
          ["n7"],
          ["n6"],
          ["n5"],
          ["n4"],
          ["n10"]
        ]
      }
   ```

   ���Է���```test_innerjoin```

   �������

   ```
	curl http://127.0.0.1:3459/test/sources/test/test_innerjoin/where/fm:eq:f/where/minId:eq:3
   ```

   ��̨�������滻��� sql ���Ϊ

   ```
   "select * from service_a.table as sa inner join service_b.table as sb on sa.id=sb.id where fm=f and minId=3"
   ```

   ���ؽ����

   ```
      { "columns" : ["id","sex","name","type"],
        "data" : [
          [4,"f","n4","t4"],
          [6,"f","n6","t6"],
          [8,"f","n8","t8"],
          [10,"f","n10","t10"]
        ]
      }
   ```
