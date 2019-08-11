# Creation-PopUp-with-SymbolicUrl
http://siebel.ittoolbox.com/groups/technical-functional/siebel-dev-l/symbolic-url-to-open-a-url-4803253

1. Создать поле на БК, возвращающее имя Symbolic URL.

2. Скопировать апплет SSO Generic System 1 Applet. Поменять ему БК на тот, который является источником данных для Symbolic URL, с него будут браться поля, необходимые для внешней аппликации. Класс должен быть CSSSWEFramePopup. Создать контроль, ссылающееся на поле из первого пункта и указать Field Retrieval Type = Symbolic URL. Поместить это поле на GUI. Остальные котнролы на GUI не нужны. В List изменить для SSOGenericSystem1 поле на поле из первого пункта.

3. На апплете, с которого будет вызваться окно создать кнопку или линк: HTML Display Mode = EncodeData; Method Invoked = ShowPopup. User properties: Popup = Имя Поп-ап апплета; Popup Dimension = 800X800. Реально размеры апплета определяются в определении Symbolic URL.

4. В администрации на поле Symbolic URL создать новую запись, задать ей SSO Disposition = Form Redirect. В Symbolic URL Arguments создать следующие записи с Argument Type = Command: FreePopup = True; FullWindow = True (для открытия апплета в новой вкладке) или PopupSize = 750x500 (для открытия апплета в новом окне)

Примечания:
1. SSO Disposition = Form Redirect предполагает отправку http запросов методом POST. Если требуется использовать метод GET, то в Symbolic URL Arguments нужно добавить еще команду с любым именеи, и со значением GetRequest.

2. ShowPopup регистрозваисим для свойства CanInvokeMethod, но регистронезависим при основной работе. Это значит, что если создать два элемента управления с Method Invoked = ShowPopup и Method Invoked = SHowPopup, то их доступностью можно упаравлять по разному, и вызывать они будут разные аплеты

3. Для использования собственного метода (не ShowPopup) на браузер скрипте в Applet_InvokeMethod или Applet_PreInvokeMethod написать следующий код:
switch (name)
	{
		case "MyMethod":
    			var ips = theApplication().NewPropertySet();
			    var ops = theApplication().NewPropertySet();
			    ips.SetProperty("SWESP", "true");
          ips.SetProperty("SWETA", "Name of popup Applet");
          ops = this.InvokeMethod("ShowPopup",ips);
          break;
   }
