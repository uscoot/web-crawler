+ 2017年11月18日
    + API初始化

## 频道管理

+ Data
    + channelSource (Integer) - 频道来源 0:缺省 1:国内频道 2:国外频道
    + channelType (Integer) - 频道类型 1:版面频道2:标签频道3:用户频道4:来源频道5：排行频道
    + channelSourceValue (String) - 频道来源值
    + channelTypeValue (String) - 频道类型值
    + targetId (Long) - 目标id，(查看详情时，根据频道类型不同，目标也不同；当channelType=4时，此字段失效，取代字段为targets)
    + channelName (String) - 频道名称
    + channelImage (String) - 频道图片
    + description (String) - 描述
    + watched (Integer) - 关注数
    + displayOrder (Integer) - 排序
    + cancellable (Integer) - 是否可以取消关注 0：不可以 1：可以
    + targets (Long[]) - 目标id数组，当查看详情,并且channelType=4的时候有效
    + enabled (Integer) - 是否可用 0：不可用 1：可用
    + modified (Date) - 修改时间
    + created (Date) - 创建时间

### 列表 [GET] /channels?filter[channelType]=1 
+ Description
    + 列表展示id,channelName,description,channelTypeValue,watched,cancellable,enabled,creator,modifier,created,modified
+ Parameters
    + channelType
    + channelName
    + id
    + enabled
+ Response 200 (application/json)

        {
            "meta": {
                "totalPages": 8,
                "totalElements": 73,
                "size": 10,
                "number": 1,
                "numberOfElements": 10,
                "first": true,
                "last": false,
                "sort": null
            },
            "links": {
                "self": "/channels?page[number]=1&page[size]=10",
                "first": "/channels?page[number]=1&page[size]=10",
                "next": "/channels?page[number]=2&page[size]=10",
                "last": "/channels?page[number]=8&page[size]=10"
            },
            "data": [
                {
                    "id": 86,
                    "targetId": 2425,
                    "channelSource": 1,
                    "channelType": 3,
                    "channelName": "米饭星2425",
                    "channelImage": null,
                    "description": null,
                    "watched": 0,
                    "displayOrder": 0,
                    "cancellable": 1,
                    "subscribed": false,
                    "channelTypeValue": "用户频道",
                    "channelSourceValue": "国内频道"
                },
                {
                    "id": 13,
                    "targetId": 8,
                    "channelSource": 1,
                    "channelType": 4,
                    "channelName": "EAW",
                    "channelImage": "http://static.budee.com/yyren/image/channel/eaw.jpg",
                    "description": null,
                    "watched": 1,
                    "displayOrder": 0,
                    "cancellable": 1,
                    "subscribed": false,
                    "host": "http://eaw.com",
                    "channelTypeValue": "来源频道",
                    "channelSourceValue": "国内频道"
                }
            ]
        }
        
### 频道详情 [GET] /channels/{id}
+ Description
    + 展示字段id,channelName,description,channelTypeValue,targetId 或者 targets,channelImage,watched,cancellable,enabled,creator,modifier,created,modified
+ Response 200 (application/json)
    
        {
            "data": {
                "id": 2,
                "targetId": 1,
                "channelSource": 1,
                "channelType": 1,
                "channelName": "产品",
                "channelImage": "http://cdn.mifanxing.com/mifan/img/default-channel.png",
                "filter": "",
                "description": "产品",
                "watched": 4,
                "displayOrder": 0,
                "cancellable": 0,
                "subscribed": false,
                "channelTypeValue": "版面频道",
                "channelSourceValue": "国内频道"
            }
        }

### 增加/修改 [POST] /channels
+ Description
    + [MUST] Authenticated
    + [MUST] ROLE_ADMIN
    + 修改时id必填，添加时id必为空
+ Parameters
    + channelSource 
    + channelType 必填
    + targetId channelType!=4时必填
    + targets channelType=4时必填
    + channelName 必填
    + channelImage 
    + description
    + watched
    + cancellable
+ 添加Response 200 (application/json)

        {
            "data":{
                "channelType":3,
                "channelName":"测试用户频道",
                "targetId":1030
            }
        }
        
        {
            "data":{
                "channelType":4,
                "channelName":"测试来源频道-Thomann",
                "targets":[11,12]
            }
        }
        
+ 修改Response 200 (application/json)

        {
            "data":{
                "id":92,
                "channelType":4,
                "channelName":"11111111测试来源频道-Thomann修改版",
                "targets":[12]
            }
        }

### 设置enabled [DELETE] /channels/{id}/{enabled}
+ Description
    + [MUST] Authenticated
    + [MUST] ROLE_ADMIN
    + [MUST] enabled = 0|1
+ Response 204 (application/json)

### 来源频道下拉框 [GET]/channels?page[size]=1000&filter[channelType]=4&filter[enabled]=1&filter[channelName:like]=%25测试%25

### 频道关联的seed下拉框 [GET]/channels/{channelId}/seeds
+ Response 200 (application/json)
    
        {
            "data": [
                {
                    "id": 16,
                    "name": "productReview"
                },
                {
                    "id": 21,
                    "name": "documents"
                }
            ]
        }
