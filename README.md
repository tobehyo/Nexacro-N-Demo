# Nexacro N Demo
> ###### 이 저장소의 데모 샘플은 투비소프트 중국 법인에서 활용할 목적으로 playnexacro에서 제공하고 있는 [Nexacro 17 demo](https://demo.nexacroplatform.com/)를 수정한 샘플입니다.
> ###### 다국어의 경우 기계번역을 사용하였으며 잘못 된 번역이 포함 되어 있습니다.

## Live Demo
[https://os.tobesoft.com/nexacrodemo](https://os.tobesoft.com/nexacrodemo)

## Getting started

- 이 저장소를 복제(Clone) 혹은 [다운로드](https://github.com/tobehyo/nexacro-n-demo/archive/refs/heads/main.zip) 합니다.       
   ``` bash
   git clone https://github.com/tobehyo/nexacro-n-demo.git
   ```
   *다운로드한 파일은 압축해제 합니다.*
- 넥사크로 스튜디오를 통해 `demo-2020.xprj` 파일을 오픈합니다.
##### 🌠Nexacro Studio Getting Started Tutorial - [http://docs.tobesoft.com/getting_started_nexacro_n_ko#1a8c8e68a182acba](http://docs.tobesoft.com/getting_started_nexacro_n_ko#1a8c8e68a182acba)

## Demo Project Information

#### 👉 백엔드 호출 URL(aka Service URL) 설정
- Application `onload` 이벤트에서 서비스 URL를 재정의하고 있습니다.
``` js
this.Application_onload = function(obj:nexacro.Application,e:nexacro.LoadEventInfo)
{
  // set service URL 
  var objEnv = nexacro.getEnvironment();	
  var urlPath = "http://localhost:8080/";	
  var strPrjPath = nexacro.getProjectPath();

  var parseURL = strPrjPath.split("/");
  hostPath = parseURL[0] + "//" + parseURL[2] + "/";
  this.webRootUrl = hostPath;

  if(strPrjPath.indexOf("os.tobesoft.com") !==-1) {
    urlPath = strPrjPath;
    this.webRootUrl = urlPath;
  }

  objEnv.services["svc"].set_url(urlPath); 	
  objEnv.services["xeni"].set_url(urlPath);  
  ...
};
```

#### 👉 Nav 메뉴 노출
- Global dataset(`gdsAllMenu`)에서 메뉴를 관리하며 `gdsAllMenu` dataset 내용을 filter 후 `gdsMenu` 저장하여 처리합니다.
``` js
this.changeLanguage = function(langCode)
{
  ...
  
  filterExpr = "";

  filterExpr += isMobile?"mobile=='1'":"desktop=='1'";
  filterExpr += isNRE?"&&nre=='1'":"&&wre=='1'";

  pThis.gdsAllMenu.filter(filterExpr);
  pThis.gdsMenu.clearData();

  for (var i=0,len=pThis.gdsAllMenu.rowcount; i<len; i++) {
    var r = pThis.gdsMenu.addRow();
    pThis.gdsMenu.copyRow(r, pThis.gdsAllMenu, i, "id=id,caption=caption_"+langCode+",level=level,upid=upid,url=url");
  }

  pThis.gdsMenu.applyChange();
  
  ...
};
```

#### 👉 다국어 처리
- form의 공통함수인 `gfnFormOnLoad` 함수를 form의 `onload` 이벤트에서 호출하며 다국어를 설정합니다.
  > #### 메세지 설정은 컴포넌트의 User Properties 항목에 messageid 추가하여 설정합니다.
``` js
nexacro.Form.prototype.gfnFormOnLoad = function () {
  this.addEventHandler("onlayoutchanged", function (obj, e) {
      nexacro.applyI18n(obj);
      var p = this.parent.parent,
          height = obj._layout_list[e.newlayout].height;
      p["mainPageOnLoad"].call(p, height);
  }, this);

  // 다국어 적용
  nexacro.applyI18n(this);

  var p = this.parent.parent,
      height = this._layout_list[this.getCurrentLayoutName()].height;
  if (this.parent.name == "divMain") {
      p["mainPageOnLoad"].call(p, height);
  } else if (this.parent.name == "divDesc") {
      p["descPageOnLoad"].call(p, height);
  }
};
```

#### 👉 Pivot Grid 다국어 메세지 변경
- `_extlib_\pivot\NxPivot.message.js` 파일에서 메세지를 변경하면 됩니다.  

#### 👉 폰트 적용
- 폰트는 Noto Sans KR 이 적용 되어있으며 변경은 넥사크로 스튜디오의 Resource의 `UserFont` 메뉴에서 NotoSans.xfont 수정하거나 새로운 user font 를 생성하여 사용할 수 있습니다.  

> User Font 생성 방법 [http://docs.tobesoft.com/development_tools_guide_nexacro_n_ko#255e13d5d5644cb9](http://docs.tobesoft.com/development_tools_guide_nexacro_n_ko#255e13d5d5644cb9)  


## Download Nexacro N XAPI in Spring Boot Demo
[Nexacro N XAPI in Spring Boot Project](https://github.com/tobehyo/nexacro-n-spring-boot) Repository  
Download [Nexacro N XAPI Demo](https://github.com/tobehyo/nexacro-n-spring-boot/archive/refs/heads/main.zip)
