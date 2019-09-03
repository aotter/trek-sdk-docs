# AotterTrek Tracker



## Guide

One successful tracker event contains `trakcer itemId`, `actionType`, `userObject` , `entityObject` , `locationObject`.

- Tracker ItemId : the unique id of this tracker event.

- actionType : the action type of this event.

  - |      action type      |                  Description                  |
    | :-------------------: | :-------------------------------------------: |
    |   kTKTTypeREAD_POST   |       when user read a post or article        |
    |  kTKTTypeVISIT_PLACE  |          when user visit some where           |
    |   kTKTTypePLAY_GAME   | when user play a interactive game or activity |
    | kTKTTypeLISTEN_MUSIC  |  when user plays a song through music player  |
    |  kTKTTypeWATCH_VIDEO  |           when user watched a video           |
    | kTKTTypeCALL_MERCHANT |                                               |
    |   kTKTTypeBUY_ITEM    |                                               |

- entity object : the object that represent the tracker event. Has different types

|      entity type      | description |
| :-------------------: | :---------: |
|   kTKEntityTypePOST   |             |
|  kTKEntityTypePLACE   |             |
|   kTKEntityTypeGAME   |             |
|  kTKEntityTypeMUSIC   |             |
|  kTKEntityTypeVIDEO   |             |
| kTKEntityTypeMERCHANT |             |
|   kTKEntityTypeITEM   |             |

- user object : the person/user invloved in this event.
- location object : the position that event occurs.
- There are steps to send a tracker event
  - Step 1. create `entity object` / `user object` / `location object` 
  - Step 2. engage above parts.
  - Step 3. (optional) update `entity object` / `user object` / `location object` for sepcific items if it's nessasary in your app's life cycle.
  - Step 4. (optional) exit the Tracker item if the event has clear end time.
  - Step 5. send the trcaker item.



## Step 1. Create Object of tracker event

```objective-c
//create entity object with type 'POST'
   NSDictionary *entityPostObject = [TKTracker helper_entityObjectWithTypePOST:@"myPostId" title:@"post title" url:@"http://agirls.aotter.net" tags:nil categories:@[@"news"] reference:nil publishedDate:nil imageUrl:nil author:@"F.D.KKK" meta:@{@"something": @"ffff"}];
   
//or create entityObject with type 'PLACE'
NSDictionary *entityObject = [TKTracker helper_entityObjectWithTypePLACE:[NSString stringWithFormat:@"myPlaceEntityId"] title:@"Taipei" url:@"" tags:@[@"city",@"play",@"Taiwan"] categories:@[@"travel"] address:@"New Taipei City" lat:102.333 lng:63.333 meta:@{@"A": @"how do you turn this on?",@"address": @"new address shoud not be seen"}];

//create user object (optional)
NSDictionary *userObject = [TKTracker helper_userObjectWithEmail:@"<current_user_email>" phone:@"<current_user_phone>" fbId:@"<current_user_fbId>" gender:@"<F or M for current User>" birthday:[NSDate date] addtionalMeta:nil];

//create location object (optional)
double user_location_lat = 191.232323;
double user_location_lng = 244.232323;
NSDictionary *locationObject = [TKTracker helper_locationObjectWithLocationId:@"<your_location_id>" title:nil url:@"" categories:@[@"user_location"] address:nil lat:user_location_lat lng:user_location_lng additonalMeta:nil];

```

## Step 2. Engage trakcer event with these parts

```objective-c
//engage trakcer item with POST type
[[TKTracker sharedAPI] trackerEngageItemWithItemId:@"myPostId" type:kTKTTypeREAD_POST userObject:userObject entityObject:entityObject locationObject:locationObject];
```

## Step3. (optional) Update the tracker items' specific part if needed

```objective-c
[[TKTracker sharedAPI] trackerUpdateItem:@"myPostId" withEntityObject:@{@"foo":@"bar"}];
[[TKTracker sharedAPI] trackerUpdateItem:@"myPostId" withUserObject:@{@"foo":@"bar"}];
[[TKTracker sharedAPI] trackerUpdateItem:@"myPostId" withLocationObject:@{@"foo":@"bar"}];
```

## Step 4. (optional) Exit tracker item if the tracker event has clear end time.

```objective-c
//when user leave the post page, which means the user is finish reading the post
[[TKTracker sharedAPI] trackerExitItem:@"myPostId"];
```

## Step 5. Send tracker items.

```objective-c
[[TKTracker sharedAPI] trackerSendItems];        
```

# Example

```objective-c
-(void)engageItemAndSend{
    NSDictionary *entityObject = [TKTracker helper_entityObjectWithTypePOST:@"myPostId" title:@"post title" url:@"http://agirls.aotter.net" tags:nil categories:@[@"news"] reference:nil publishedDate:nil imageUrl:nil author:nil];
    NSDictionary *userObject = [TKTracker helper_userObjectWithEmail:@"my@email.net" phone:@"0922333444" fbId:@"" gender:@"M" birthday:@"1999/05/02" addtionalMeta:nil];
    [[TKTracker sharedAPI] trackerEngageItemWithItemId:@"myPostId" type:kTKTTypeREAD_POST userObject:userObject entityObject:entityObject locationObject:nil];
    dispatch_after(dispatch_time(DISPATCH_TIME_NOW, (int64_t)(2 * NSEC_PER_SEC)), dispatch_get_main_queue(), ^{
        [[TKTracker sharedAPI] tTrackerExitItem:@"myPostId"];
        [[TKTracker sharedAPI] trackerSendItems];
    });
```