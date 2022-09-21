# GOOGLE API for translation

### <br/><br/><br/>

###############################################################################################################################################
### google cloud 웹에서 사용하는 방법 start
### <br/><br/><br/>

## 가입
### google cloud 에 가서 가입한다. 처음 가입가면 300 달러의 무료 크레딧을 준다.
#### https://cloud.google.com/

### <br/><br/><br/>

## 준비
### 검색에서 translation 이라고 검색할 후 튜토리얼 문서를 참고한다. python 으로 하든 node.js 로 하든 거의 비슷하고 나중에 코드 가져오고 실행하는 것만 다르게 해주면 된다.
![image](https://user-images.githubusercontent.com/62974484/191546771-c0229fec-53e5-4989-9408-2b05f8b17c83.png)
### 다음을 따라한다.
```
# 디렉토리 만들고 환경 변수 설정
mkdir translate-nodejs && touch \
    translate-nodejs/app.js
    
cd translate-nodejs
cloudshell open-workspace .

export \
    PROJECT_ID=key-partition-363207
    
# API 인증
gcloud iam service-accounts create \
    translate-quickstart --project \
    key-partition-363207
    
# API 사용자 역할 부여
gcloud projects \
    add-iam-policy-binding \
    key-partition-363207 --member \
    serviceAccount:translate-quickstart@key-partition-363207.iam.gserviceaccount.com \
    --role roles/cloudtranslate.user
    
# 서비스 계정 키 만들기
gcloud iam service-accounts keys \
    create translate-key.json \
    --iam-account \
    translate-quickstart@key-partition-363207.iam.gserviceaccount.com
    
# 이 키를 사용자 기본 인증 계정 키로 설정
export \
    GOOGLE_APPLICATION_CREDENTIALS=translate-key.json
    
# translation 을 하기 위한 모듈 설치
pip3 install google-cloud-translate
```


### <br/><br/><br/> 

## 번역 실행
### 다음의 튜토리얼 웹사이트를 참고한다.
#### https://cloud.google.com/translate/docs/basic/translating-text?hl=ko#translate_translate_text-python
### API 문서를 참고할 수 있는 다른 방법(이 방법이 더 깔끔)
![image](https://user-images.githubusercontent.com/62974484/191549404-42a04f2a-e60b-4c8e-93af-bac7b43fe1b8.png) <br/> 
![image](https://user-images.githubusercontent.com/62974484/191549593-f96bf830-583b-4b99-b00d-fe6e2b78800f.png) <br/> 
![image](https://user-images.githubusercontent.com/62974484/191549671-06d42370-2b02-40ce-9912-bff1e3b131c0.png) <br/> 
![image](https://user-images.githubusercontent.com/62974484/191550023-84da8dd1-8f5f-45e4-bfd6-64f5db8072ab.png) <br/> 
![image](https://user-images.githubusercontent.com/62974484/191550179-41d59458-1f6b-4737-9289-25a2f72fbb88.png) <br/> 
![image](https://user-images.githubusercontent.com/62974484/191550290-715620fe-972a-40a2-a269-74ec9dc8f8a5.png) <br/> 
### 코드 복사한 후 사용한다. 위에서 복사한 코드의 경우에는 source_language (원래 언어)를 자동인식해주지만 설정할 수 있다.
### source_language 를 파라미터로 추가해서 사용한다.
### 주의 사항에 써져 있 듯이 ISO 639-1 language code 를 따른다.

### 번역 실행 코드 (source_language 포함)
```
# [START translate_text_with_model]
def translate_text_with_model(target, source_language, text, model="nmt"):
    """Translates text into the target language.

    Make sure your project is allowlisted.

    Target must be an ISO 639-1 language code.
    See https://g.co/cloud/translate/v2/translate-reference#supported_languages
    """
    import six
    from google.cloud import translate_v2 as translate

    translate_client = translate.Client()

    if isinstance(text, six.binary_type):
        text = text.decode("utf-8")

    # Text can also be a sequence of strings, in which case this method
    # will return a sequence of results for each text.
    result = translate_client.translate(text, target_language=target, source_language = source_language, model=model)

    print(u"Text: {}".format(result["input"]))
    print(u"Translation: {}".format(result["translatedText"]))
    #print(u"Detected source language: {}".format(result["detectedSourceLanguage"]))


# [END translate_text_with_model]

translate_text_with_model("ko", "en", "This is my first translation.", model="nmt")

# 
```

### <br/><br/><br/>

## 결과
![image](https://user-images.githubusercontent.com/62974484/191551529-e14fc825-2345-4f78-bec4-1fa06c49f117.png)


### google cloud 웹에서 사용하는 방법 end
###############################################################################################################################################
