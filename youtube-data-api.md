# youtube-data-api
- channel `categoryId` 는 폐기됨
  > guideCategory 리소스는 채널의 콘텐츠 또는 채널의 인기도와 같은 기타 지표를 기반으로 YouTube에서 알고리즘을 통해 할당하는 카테고리를 식별합니다. 이 목록은 동영상 카테고리와 유사하지만, 동영상 카테고리는 동영상 업로더가 할당할 수 있지만 채널 카테고리는 YouTube만 할당할 수 있다는 점이 다릅니다.
- videoCategory
  - `regionCode` 지정은 video 의 카테고리를 얻어 올 수 있음
- channel.topicDetails
  - topic 이 사실상 채널 카테고리
  - brandingSettings,contentDetails,contentOwnerDetails,localizations,snippet,statistics,status,topicDetails
    ```json
    {
      "kind": "youtube#channelListResponse",
      "etag": "khzuzMRLvoMd5LMUXru5xylFDOs",
      "pageInfo": {
        "totalResults": 1,
        "resultsPerPage": 5
      },
      "items": [
        {
          "kind": "youtube#channel",
          "etag": "uvNU2fpn1xdiD1KtlO1rgVSemXE",
          "id": "UC_x5XG1OV2P6uZZ5FSM9Ttw",
          "snippet": {
            "title": "Google for Developers",
            "description": "Subscribe to join a community of creative developers and learn the latest in Google technology — from AI and cloud, to mobile and web.\n\nExplore more at developers.google.com\n\n",
            "customUrl": "@googledevelopers",
            "publishedAt": "2007-08-23T00:34:43Z",
            "thumbnails": {
              "default": {
                "url": "https://yt3.ggpht.com/2eI1TjX447QZFDe6R32K0V2mjbVMKT5mIfQR-wK5bAsxttS_7qzUDS1ojoSKeSP0NuWd6sl7qQ=s88-c-k-c0x00ffffff-no-rj",
                "width": 88,
                "height": 88
              },
              "medium": {
                "url": "https://yt3.ggpht.com/2eI1TjX447QZFDe6R32K0V2mjbVMKT5mIfQR-wK5bAsxttS_7qzUDS1ojoSKeSP0NuWd6sl7qQ=s240-c-k-c0x00ffffff-no-rj",
                "width": 240,
                "height": 240
              },
              "high": {
                "url": "https://yt3.ggpht.com/2eI1TjX447QZFDe6R32K0V2mjbVMKT5mIfQR-wK5bAsxttS_7qzUDS1ojoSKeSP0NuWd6sl7qQ=s800-c-k-c0x00ffffff-no-rj",
                "width": 800,
                "height": 800
              }
            },
            "localized": {
              "title": "Google for Developers",
              "description": "Subscribe to join a community of creative developers and learn the latest in Google technology — from AI and cloud, to mobile and web.\n\nExplore more at developers.google.com\n\n"
            },
            "country": "US"
          },
          "contentDetails": {
            "relatedPlaylists": {
              "likes": "",
              "uploads": "UU_x5XG1OV2P6uZZ5FSM9Ttw"
            }
          },
          "statistics": {
            "viewCount": "311925330",
            "subscriberCount": "2420000",
            "hiddenSubscriberCount": false,
            "videoCount": "6463"
          },
          "topicDetails": {
            "topicIds": [
              "/m/019_rr",
              "/m/07c1v",
              "/m/01k8wb"
            ],
            "topicCategories": [
              "https://en.wikipedia.org/wiki/Lifestyle_(sociology)",
              "https://en.wikipedia.org/wiki/Technology",
              "https://en.wikipedia.org/wiki/Knowledge"
            ]
          },
          "status": {
            "privacyStatus": "public",
            "isLinked": true,
            "longUploadsStatus": "longUploadsUnspecified",
            "madeForKids": false
          },
          "brandingSettings": {
            "channel": {
              "title": "Google for Developers",
              "description": "Subscribe to join a community of creative developers and learn the latest in Google technology — from AI and cloud, to mobile and web.\n\nExplore more at developers.google.com\n\n",
              "keywords": "\"google developers\" developers \"Google developers videos\" \"google developer tutorials\" \"developer tutorials\" \"developer news\" android firebase tensorflow chrome web flutter \"google developer experts\" \"google launchpad\" \"developer updates\" google \"google design\"",
              "trackingAnalyticsAccountId": "YT-9170156-1",
              "unsubscribedTrailer": "bC8fvcpocBU",
              "country": "US"
            },
            "image": {
              "bannerExternalUrl": "https://yt3.googleusercontent.com/aG8yBYiZrMEPIw8pwusgPjzdPPccrbKqa1JVDs0PNPvj3X45Pr0wPRYBlIdufxn-rqhOKWCR"
            }
          },
          "contentOwnerDetails": {}
        }
      ]
    }
    ```
## link
- [[youtube]]
- [[api]]
