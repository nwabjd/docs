---

copyright:
  years: 2016, 2017
lastupdated: "2017-02-02"
---

{:new_window: target="\_blank"}
{:shortdesc: .shortdesc}
{:screen:.screen}
{:codeblock:.codeblock}
{:pre: .pre}

# 인증서 구성
{: #set_up_certificates}

인증서는 디바이스 인증에 사용되거나 MQTT 메시징에 대한 기본 {{site.data.keyword.iot_full}} 서버 인증서를 대체하는 데 사용됩니다. 서명된 유효한 인증서가 없는 디바이스는 액세스가 거부되고 서버와 통신할 수 없습니다. 

디바이스에 대해 인증서 및 서버 액세스 권한을 구성하기 위해 시스템 운영자는 연관된 인증 기관(CA) 인증서를 등록하며, 선택적으로 메시지 서버 인증서를 {{site.data.keyword.iot_short_notm}} 플랫폼에 등록합니다. 

## 인증서 요구사항
{: #cert_reqs}

### CA 인증서
조직은 CA 인증서를 사용하여 해당 디바이스가 서버에 연결될 수 있도록 디바이스의 클라이언트 인증서가 신뢰 가능하다고 인식할 수 있습니다. CA 인증서는 인증서 폐기 목록(CRL)을 사용할 수 있습니다. CA가 플랫폼에 추가되는 시점에는 CRL에 접근할 수 있어야 하며, 그렇지 않으면 CA 인증서가 거부됩니다. 

CA 인증서를 추가하거나 메시징 서버 인증서를 대체하는 경우, 모든 디바이스는 서버가 적합한 CA를 사용하여 디바이스를 인증할 수 있도록 SNI(Server Name Indication)를 지원하는 MQTT 클라이언트를 사용하여 연결해야 합니다. 

CA 인증서를 추가하지만 위험 및 보안 관리 추가 기능이 사용되지 않는 경우, 모든 디바이스는 TLS와 토큰 및 클라이언트 인증서 인증 연결 정책으로 연결합니다. 위험 및 보안 관리 추가 기능이 사용되는 경우에는 다양한 연결 정책을 구성할 수 있습니다. 연결 보안 정책을 구성하는 방법에 대한 정보는 [보안 정책 구성](set_up_policies.html)을 참조하십시오. 

### 클라이언트 또는 디바이스 인증서
개별 클라이언트 또는 디바이스 인증서는 디바이스에 남아 있으며 플랫폼으로 업로드되지 않습니다. 모든 디바이스 인증서를 서명하는 데 사용되는 CA 서명 인증서가 플랫폼에 업로드되는 유일한 인증서입니다. 자체 서명된 서버 인증서를 사용 중인 경우에는 클라이언트 인증서(cert.pem)의 서명에 사용되는 루트 및 중간 CA(ca.pem)를 업로드해야 합니다. 

CA 인증서로 서명한 개별 디바이스 인증서는 인증서에 CN(Common Name) 또는 SubjectAltName으로 입력된 디바이스 ID가 있어야 합니다. *CN* 필드의 경우 형식은 'CN=d:devtype:devid'입니다. SubjectAltName 필드의 경우 형식은 'SubjectAltName=email:d:devtype:devid'입니다. 여기서 'devtype'은 디바이스의 디바이스 유형이며, 'devid'는 디바이스의 클라이언트 ID입니다. 

## 디바이스 인증을 위한 인증 기관(CA) 인증서 등록
{: #reg_ca_cert}

1. {{site.data.keyword.iot_short_notm}}에 로그인하고 **일반 설정**으로 이동하십시오. 
2. **보안** 섹션의 **CA 인증서** 아래에서 **인증서 추가**를 클릭하십시오. 
3. 찾아보기를 사용하여 업로드할 인증서 파일을 선택하거나 **인증서 추가** 창에서 파일을 끌어서 놓으십시오. 파일에는 인증서가 하나만 포함될 수 있으며 인증서 날짜가 유효해야 합니다. .pem 또는 .der 형식의 인증서만 허용됩니다. 선택된 파일 내의 인증서 정보를 미리볼 수 있습니다. 
4. 인증서 파일에 대한 설명을 입력하십시오. 
5. 올바른 파일이 선택되었는지 확인한 후 **저장**을 클릭하십시오. 선택된 인증서는 테이블에 나열되며, 기본적으로 활성화되어 있습니다. 

## 메시징 서버 인증서 등록
{: #reg_msg_cert}

기본 서버 인증서는 플랫폼에서 제공됩니다. 기본 인증서를 사용하거나 사용자 조직의 인증서를 업로드할 수 있습니다. 아직 사용할 인증서가 없는 경우에는 새 인증서에 대한 요청을 작성할 수 있습니다. 새 인증서를 수신하면 서명 후에 다시 플랫폼에 업로드해야 합니다. 

기본 인증서를 사용하거나 이미 업로드된 다른 인증서를 사용하려면 **메시징 서버 인증서** 아래의 테이블에 있는 **기본 메시징 서버 인증서** 드롭 다운 목록에서 사용하고자 하는 인증서를 선택하십시오. 

**참고:** 플랫폼 대시보드 페이지가 메시징 서버에 대한 내부 연결을 수행하여 디바이스 정보를 검색할 수 있습니다. 브라우저가 기본적으로 메시징 서버를 신뢰 서버로 인식하지 않으므로, 조직에 대해 자체 서명된 서버 인증서를 설정할 때 대시보드 사용자는 연결 문제를 피하기 위해 자체 브라우저에서 신뢰 인증서로서 서버 인증서를 추가해야 합니다. 

### 사용자 조직의 인증서 업로드
{: #upload_cert}
1. **일반 설정**의 **보안** 섹션의 **메시징 서버 인증서** 아래에서 **인증서 추가**를 클릭하십시오. 
2. 찾아보기를 사용하여 업로드할 인증서 파일을 선택하거나 **인증서 추가** 창에서 파일을 끌어서 놓으십시오. 
3. 찾아보기를 사용하여 업로드할 개인 키 파일을 선택하거나 **인증서 추가** 창에서 파일을 끌어서 놓으십시오.   
4. 개인 키가 비밀번호 문구로 암호화된 경우 개인 키의 비밀번호 문구를 입력하십시오.
5. 올바른 파일이 선택되었는지 확인한 후 **저장**을 클릭하십시오. 
6. **기본 메시징 서버 인증서** 드롭 다운 목록에서 새로 업로드한 인증서를 선택하십시오. 선택된 인증서가 활성 인증서로서 테이블에 나열됩니다. 

### 새 인증서 요청
{: #request_cert}

새 메시징 서버 인증서를 사용하려면 인증서 서명 요청(CSR)을 생성하여 새 인증서를 요청할 수 있습니다. 

 1. **일반 설정**의 **보안** 섹션의 **메시징 서버 인증서** 아래에서 **CSR 생성**을 클릭하십시오. 
 2. 서버에 대한 CSR을 요청하기 위한 세부사항을 입력하고 **생성**을 클릭하십시오. 테이블에 CSR이 표시됩니다. 
 3. 요청을 다운로드하여 서명을 위해 인증 기관에 제출하십시오. 
 4. 인증서를 가져온 후에는 [사용자 조직의 인증서 업로드](#upload_cert)의 단계에 따라 이를 업로드할 수 있습니다. 인증서가 업로드되면 테이블의 CSR이 업로드된 인증서로 대체됩니다. 
