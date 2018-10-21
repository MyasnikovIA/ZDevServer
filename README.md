# ZDevServer
Сервер/ Клиент для прямого обмена данными между серверами, по сокет подключению
В XML файле есть примеры применеия
<br>Запустить сервер:    d ##class(%ZDev.Server).Start(6030,"USER")
<br>Остановить сервер:   d ##class(%ZDev.Server).Stop(6030)

<h2> Подключится к удаленному серверу, отправить команду и получить ответ в виде текста</h2>
<pre>
    s obj=##class(%ZDev.Client).%New()
    if obj.Connect("localhost",6030,"_SYSTEM","SYS","Sirena",.Error)=1 { 
        d obj.Push("ip")
        w !,obj.ReadLine()
        d obj.Push("w $ZDT($h)")
        d obj.Push("run")
        s str=obj.Read()
        w !,str
    }else{ 
       zw Error	
    }
    d obj.DisConnect()
    s obj=""
  </pre>
  
<h2>Подключится к серверу, скопировать код метода на удаленный компьютер и создать там *.MAC программу. После чего выполнить эту программу</h2>
<pre>
Class %ZDev.Demo.demo2RunMethod Extends %RegisteredObject
{

ClassMethod run()
{
       s obj=##class(%ZDev.Client).%New()
       if obj.Connect("ne1234",6030,"IRISUserDev","SYS","Sirena",.Error)=1 {
	    d obj.RunClassMethod(##this,"GetTime",.Error,$zu(5) ) 
	    s AtEnd=0
	    while AtEnd=0 {
	      s str=obj.ReadLine(.AtEnd,.Error)
	      w !,str
	    }
	}else{
	  zw Error	
	}
	d obj.DisConnect()
	s obj=""
}

Method GetTime(Arg1 = "TestArg")
{ 
    w $h,!
    w Arg1,!
    w $ZU(110),!
}

}
</pre>
