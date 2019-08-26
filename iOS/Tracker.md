# AotterTrek Tracker



## Guide

tracker物件須包含 `itemId`, `actionType`, `userObject`, `entityObject`, `locationObject` 等物件。 傳送一個trakcer item需要經過下列步驟：

- Step 1. engage the tracker item.
- Step 2. update the trakcer item (optional).
- Step 3. exit the trakcer item (optional).
- Step 4. send the trakcer items.

## Step 1. engage tracker item.

1. 一個tracker item由 `entity`, `user`, `location`等部件組合，可以透過 `[TKTracker helper_]`系列來取得正確的format資料
2. 如果在初始化的時間點，上述三個object還無法產生或是不完整，可以先用nil代入，在Step 2的時候再進行更新即可。
3. 若要紀錄timespan，請必填itemId

```objective-c
-(void)trackerEngageItemWithItemId:(NSString *)itemId
                                type:(NSString *)type
                          userObject:(NSDictionary *)userObject
                        entityObject:(NSDictionary *)entityObject
                      locationObject:(NSDictionary *)locationObject;
```

1. 若沒有itemId，則可使用以下：會自動產生itemid，但就不能再修改/刪除/紀錄timespan此tracker item

```objective-c
-(void)trackerEngageItemWithType:(NSString *)type
                          userObject:(NSDictionary *)userObject
                        entityObject:(NSDictionary *)entityObject
                      locationObject:(NSDictionary *)locationObject;
```

### Step 1-1 entity item

```objective-c
+(NSDictionary *)helper_customEntityObjectWithType:(NSString *)type
                                          entityId:(NSString *)entityId
                                             title:(NSString *)title
                                               url:(NSString *)url
                                              tags:(NSArray *)tags
                                        categories:(NSArray *)categories
                                              meta:(NSDictionary *)meta;
```

1. item type是純字串，可以自定義，不同的type會支援不同的meta格式，建議使用 `kTKTTypeREAD_POST`, `kTKTTypeVISIT_PLACE`, `kTKTTypePLAY_GAME` etc..，定義在TKTracker.h中
2. 或是使用 `helper_entityObjectWithTypePOST`, `helper_entityObjectWithTypePLACE` 來取得正確的entity format資料

```objective-c
+(NSDictionary *)helper_entityObjectWithTypePOST:(NSString *)entityId
                                           title:(NSString *)title
                                             url:(NSString *)url
                                            tags:(NSArray *)tags
                                      categories:(NSArray *)categories
                                       reference:(NSString *)reference
                                   publishedDate:(NSDate *)publishedDate
                                        imageUrl:(NSString *)imageUrl
                                          author:(NSString *)author;

+(NSDictionary *)helper_entityObjectWithTypePLACE:(NSString *)entityId
                                           title:(NSString *)title
                                             url:(NSString *)url
                                            tags:(NSArray *)tags
                                       categories:(NSArray *)categories
                                          address:(NSString *)address
                                              lat:(double)lat
                                              lng:(double)lng;
```

### Step 1-2. User Object

1. meta資料可以自定義，須為JSON格式的NSDictionary

```objective-c
+(NSDictionary *)helper_userObjectWithEmail:(NSString *)email
                                     phone:(NSString *)phone
                                      fbId:(NSString *)fbId
                                    gender:(NSString *)gender
                                  birthday:(NSDateComponents *)birthday
                             addtionalMeta:(NSDictionary *)meta;

### Step 1-3 Location Object ###
1. meta資料可以自定義，須為JSON格式的NSDictionary
+(NSDictionary *)helper_locationObjectWithType:(ATTrackerType)type
                                    locationId:(NSString *)locationId
                                         title:(NSString *)title
                                           url:(NSString *)url
                                    categories:(NSArray *)categories
                                       address:(NSString *)address
                                           lat:(double)lat
                                           lng:(double)lng
                                 additonalMeta:(NSDictionary *)meta;
```

## Step 2. update tracker item (optional)

1. 對特定的tracker item進行 `type`, `userObject`, `locationObject`, `entityObject`進行更新

```objective-c
-(void)trackerUpdateItem:(NSString *)itemId withType:(NSString *)type;
-(void)trackerUpdateItem:(NSString *)itemId withUserObject:(NSDictionary *)userObject;
-(void)trackerUpdateItem:(NSString *)itemId withEntityObject:(NSDictionary *)entityObject;
-(void)trackerUpdateItem:(NSString *)itemId withLocationObject:(NSDictionary *)locationObject;
```

## Step 3. exit the trakcer item (optional)

1. 對特定的tracker item標記exit，若是此筆資料有紀錄tracker item id，此舉會產生timespan資料。

```objective-c
-(void)trackerExitItem:(NSString *)itemId;
```

## Step 4. send the tracker item

1. 將所有tracker item送出
2. tracker item不一定要每一個都step by step得engage-exit-send，可以累積複數個tracker item之後再用send item一次全部送出。端看app設計使用情境方便即可

```objective-c
-(void)trackerSendItems;

```

# Example

```objective-c
-(void)engageItemAndSend{
    NSDictionary *entityObject = [TKTracker helper_entityObjectWithTypePOST:@"myPostId" title:@"文章標題" url:@"http://agirls.aotter.net" tags:nil categories:@[@"news"] reference:nil publishedDate:nil imageUrl:nil author:nil];
    NSDictionary *userObject = [TKTracker helper_userObjectWithEmail:@"my@email.net" phone:@"0922333444" fbId:@"" gender:@"M" birthday:@"1999/05/02" addtionalMeta:nil];
    [[TKTracker sharedAPI] trackerEngageItemWithItemId:@"myPostId" type:kTKTTypeREAD_POST userObject:userObject entityObject:entityObject locationObject:nil];
    dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(2 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
        [[TKTracker sharedAPI] tTrackerExitItem:@"myPostId"];
        [[TKTracker sharedAPI] trackerSendItems];
    });
```