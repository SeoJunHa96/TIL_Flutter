# Flutter_API 활용방법

(출처 :https://velog.io/@tygerhwang)



- dependencies

  pub.dev에서 'http'다운로드해서 적용하기



- Model

  API 데이터를 가져올 때 json 형태의 구조를 앱에서 사용할 객체로 변환해야 한다.

  ```dart
  class PiscumPhotoModel {
    final String id;
    final String author;
    final int width;
    final int height;
    final String url;
    final String downloadUrl;
  
    PiscumPhotoModel({
      required this.id,
      required this.author,
      required this.width,
      required this.height,
      required this.url,
      required this.downloadUrl,
    });
  }
  ```

  ```dart
   PiscumPhotoModel.fromJson(Map<String, dynamic> json)
        : id = json["id"],
          author = json["author"],
          width = json["width"],
          height = json["height"],
          url = json["url"],
          downloadUrl = json["download_url"];
  ```

  

- UI

  API에서 받아온 모델을 '리스트뷰'를 통해 보여준다.

  ```dart
  body: NotificationListener<ScrollUpdateNotification>(
                onNotification: (ScrollUpdateNotification notification) {
                  state.scrollListerner(notification);
                  return false;
                },
                child: ListView.builder(
                    itemCount: state.photos.length,
                    itemBuilder: ((context, index) {
                      return Padding(
                        padding: const EdgeInsets.symmetric(
                            horizontal: 20, vertical: 8),
                        child: Column(
                          children: [
                            SizedBox(
                              child: Row(
                                crossAxisAlignment: CrossAxisAlignment.start,
                                children: [
                                  SizedBox(
                                    width:
                                        MediaQuery.of(context).size.width * 0.2,
                                    height:
                                        MediaQuery.of(context).size.width * 0.2,
                                    child: ClipRRect(
                                      borderRadius: BorderRadius.circular(12),
                                      child: Image.network(
                                        state.photos[index].downloadUrl,
                                        fit: BoxFit.cover,
                                        frameBuilder: (BuildContext context,
                                            Widget child,
                                            int? frame,
                                            bool wasSynchronouslyLoaded) {
                                          return Container(
                                            decoration: BoxDecoration(
                                              borderRadius:
                                                  BorderRadius.circular(12),
                                              color: const Color.fromRGBO(
                                                  91, 91, 91, 1),
                                            ),
                                            child: child,
                                          );
                                        },
                                        loadingBuilder: (BuildContext context,
                                            Widget child,
                                            ImageChunkEvent? loadingProgress) {
                                          if (loadingProgress == null) {
                                            return child;
                                          }
                                          return Container(
                                            decoration: BoxDecoration(
                                              borderRadius:
                                                  BorderRadius.circular(12),
                                              color: const Color.fromRGBO(
                                                  91, 91, 91, 1),
                                            ),
                                            child: const Center(
                                              child: CircularProgressIndicator(
                                                color: Colors.amber,
                                              ),
                                            ),
                                          );
                                        },
                                      ),
                                    ),
                                  ),
                                  const SizedBox(width: 12),
                                  Column(
                                    crossAxisAlignment: CrossAxisAlignment.start,
                                    children: [
                                      _content(
                                          url: state.photos[index].url,
                                          title: "ID : ",
                                          content: state.photos[index].id),
                                      _content(
                                          url: state.photos[index].url,
                                          title: "Author : ",
                                          content: state.photos[index].author),
                                      _content(
                                          url: state.photos[index].url,
                                          title: "Width : ",
                                          content:
                                              "${state.photos[index].width}"),
                                      _content(
                                          url: state.photos[index].url,
                                          title: "Height : ",
                                          content:
                                              "${state.photos[index].height}"),
                                    ],
                                  )
                                ],
                              ),
                            ),
                            if (state.photos.length - 1 == index &&
                                state.isAdd) ...[
                              const SizedBox(
                                height: 100,
                                child: Center(
                                    child: CircularProgressIndicator(
                                  color: Colors.deepOrange,
                                )),
                              ),
                            ],
                          ],
                        ),
                      );
                    })),
              ),
  ```

  ```dart
  GestureDetector _content({
      required String title,
      required String content,
      required String url,
    }) {
      return GestureDetector(
        onTap: () async {
          if (await canLaunchUrlString(url)) {
            await launchUrlString(url, mode: LaunchMode.externalApplication);
          }
        },
        child: Row(
          children: [
            Text(
              title,
              style: const TextStyle(
                fontSize: 14,
                fontWeight: FontWeight.bold,
              ),
            ),
            Text(
              content,
              style: const TextStyle(
                  fontSize: 14, color: Color.fromRGBO(215, 215, 215, 1)),
            ),
          ],
        ),
      );
    }
  ```

  

- Provider

  상태관리

  API를 통해 받아온 모델의 객체를 담고 있는 리스트

  현재 어떤 페이지를 호출한지에 대한 상태

  UI에 로딩을 표시할 불리언 상태가 있음

  ```dart
  class HttpProvider extends ChangeNotifier {
  	List<PiscumPhotoModel> photos = [];
  	int currentPageNo = 1;
  	bool isAdd = false;
      ...
    }
  ```


  페이지 진입시 API호출을 통해 데이터를 세팅

  ```dart
    Future<void> started() async {
      await _getPhotos();
    }
  ```

  API로부터 데이터를 받아와 변수를 넣어주고

  1번 페이지를 호출한 후 페이지 변수 변경

  ```dart
  Future<void> _getPhotos() async {
      List<PiscumPhotoModel>? _data = await _fetchPost(pageNo: currentPageNo);
      photos = _data;
      currentPageNo = 2;
      logger.e(currentPageNo);
      notifyListeners();
    }
  ```



- API호출

  async-await 키워드를 사용

  API 응답코드가 200일 경우 성공코드

  ```dart
    import 'package:http/http.dart' as http;
  
    Future<List<PiscumPhotoModel>> _fetchPost({
      required int pageNo,
    }) async {
      try {
        http.Response _response = await http.get(
            Uri.parse("https://picsum.photos/v2/list?page=$pageNo&limit=10"));
        if (_response.statusCode == 200) {
          List<dynamic> _data = json.decode(_response.body);
          List<PiscumPhotoModel> _result =
              _data.map((e) => PiscumPhotoModel.fromJson(e)).toList();
          return _result;
        } else {
          return [];
        }
      } catch (error) {
        logger.e(error);
        return [];
      }
    }
  ```


  무한 스크롤로 API데이터를 계속 호출해주기 위해서 스크롤 포지션 값을 가져오는 코드

  ```dart
   void scrollListerner(ScrollUpdateNotification notification) {
      if (notification.metrics.maxScrollExtent * 0.85 <
          notification.metrics.pixels) {
        _morePhotos();
      }
    }
  ```

  무한 스크롤에서 중복호출 되지 않도록 하기 위한 코드

  ```dart
   Future<void> _morePhotos() async {
      if (!isAdd) {
        isAdd = true;
        notifyListeners();
        List<PiscumPhotoModel>? _data = await _fetchPost(pageNo: currentPageNo);
        Future.delayed(const Duration(milliseconds: 1000), () {
          photos.addAll(_data);
          currentPageNo = currentPageNo + 1;
          isAdd = false;
          notifyListeners();
        });
      }
    }
  ```

  

