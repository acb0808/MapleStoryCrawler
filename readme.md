# 메이플스토리 크롤러

## 프로젝트 소개
이 프로젝트는 메이플 스토리에 대한 정보를 크롤링해주는 파이썬 패키지입니다. 현재는 이벤트와 관련된 클래스만 구현되어 있으며, 추후에 더 많은 클래스가 추가될 예정입니다.

추후 디스코드 봇을 만들기 위해 사용될 예정입니다.

## License

**MIT License**

## 클래스 및 함수 설명

### Event 클래스

Event 클래스는 이벤트에 대한 정보를 표현하는 클래스입니다.

#### 변수

- `info`: 이벤트 정보를 담고 있는 리스트입니다. 인덱스 순서는 다음과 같습니다:
    - 인덱스 0: 이벤트 URL
    - 인덱스 1: 이벤트 썸네일 URL
    - 인덱스 2: 이벤트 제목
    - 인덱스 3: 이벤트 기간

- `url`: 이벤트의 URL을 저장하는 변수입니다.
- `thumbnail`: 이벤트 썸네일의 URL을 저장하는 변수입니다.
- `title`: 이벤트 제목을 저장하는 변수입니다.
- `date`: 이벤트 기간을 저장하는 변수입니다.
- `id`: 이벤트의 고유 ID를 저장하는 변수입니다.

#### 메서드

- `__init__(self, info)`: 클래스를 초기화하는 생성자 함수로, `info` 리스트를 입력으로 받습니다.
    - 입력인자
        - `info` (required, [URL:str, thumbnail:str, title:str, date:str])
- `__repr__(self)`: 객체를 문자열로 표현하는 메서드로, `info` 리스트를 문자열로 반환합니다.
- `view(self)`: 이벤트의 상세 정보를 크롤링하여 저장하는 메서드입니다. 이벤트 내용, 이미지를 저장합니다.

### EventBundle 클래스

EventBundle 클래스는 이벤트 정보를 크롤링하는 기능을 제공하는 클래스입니다.

#### 메서드

- `__init__(self)`: 클래스를 초기화하는 생성자 함수로, 초기화 메시지를 출력합니다.
- `connection_check(self, host='http://8.8.8.8')`: 인터넷 연결을 확인하는 메서드로, `host` 인자를 입력으로 받습니다. 기본값은 `http://8.8.8.8`로 설정되어 있습니다.
    - 입력 인자:
        - `host` (str, optional): 인터넷 연결을 확인할 호스트 주소입니다. 기본값은 `http://8.8.8.8`입니다. 구글의 호스트 서버이므로 바꾸는 것을 추천드립니다.
    - 반환값:
        - (bool): 인터넷 연결 가능 여부를 나타내는 불리언 값으로, True는 연결 가능, False는 연결 불가능을 나타냅니다.
- `getOngoingEvent(self, page=1)`: 진행 중인 이벤트 정보를 크롤링하는 메서드입니다. `page` 인자를 입력으로 받습니다.
    - 입력 인자:
        - `page` (int, optional): 이벤트 페이지 번호입니다. 기본값은 1입니다.
    - 반환값:
        - (list): 이벤트 정보를 담은 Event 객체의 리스트를 반환합니다.
- `getClosedEvent(self, page=1)`: 종료된 이벤트 정보를 크롤링하는 메서드입니다. `page` 인자를 입력으로 받습니다.
    - 입력 인자:
        - `page` (int, optional): 이벤트 페이지 번호입니다. 기본값은 1입니다.
    - 반환값:
        - (list): 이벤트 정보를 담은 Event 객체의 리스트를 반환합니다.

## 테스트 코드 설명


```py
handler = EventBundle()
if handler.connection_check('https://maplestory.nexon.com'):
    latest_event = handler.getOngoingEvent(page=1)[0]
    latest_event.view()
    print('이벤트 제목 : ', latest_event.title)
    print('이벤트 기간 : ', latest_event.date)
    print('이벤트 썸네일 : ', latest_event.thumbnail)
    print('이벤트 보기 url : ', latest_event.url)
    print('이벤트 내용 사진 : ', latest_event.img)

    print('\n === 이벤트 내용 ===')
    print(latest_event.content)
```

테스트 코드는 EventBundle 클래스를 초기화하고, 인터넷 연결 가능 여부를 확인한 뒤 진행 중인 이벤트 중 가장 최근 이벤트를 가져와서 상세 정보를 출력합니다.

- `handler = EventBundle()`: EventBundle 클래스를 초기화하여 객체 `handler`를 생성합니다.
- `if handler.connection_check('https://maplestory.nexon.com'):`: `handler` 객체의 `connection_check` 메서드를 사용하여 메이플 스토리 웹사이트와의 연결 가능 여부를 확인합니다.
- `latest_event = handler.getOngoingEvent(page=1)[0]`: `handler` 객체의 `getOngoingEvent` 메서드를 사용하여 첫 번째 페이지의 진행 중인 이벤트 중 가장 최근 이벤트를 가져옵니다. 가져온 이벤트는 `latest_event` 객체에 저장됩니다.
- `latest_event.view()`: `latest_event` 객체의 `view` 메서드를 호출하여 이벤트의 상세 정보를 크롤링하고, 해당 정보를 객체에 저장합니다.
- 이후에 `latest_event` 객체의 속성들인 `title`, `date`, `thumbnail`, `url`, `img`, `content` 등을 사용하여 이벤트의 제목, 기간, 썸네일, URL, 내용 등을 출력합니다.

테스트 코드의 출력값은 주어진 코드에 따라 다를 수 있으며, 2023-07-22 기준으로 썬데이 메이플 이벤트에 대한 정보가 출력되었습니다.

