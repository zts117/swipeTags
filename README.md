# swipeTags
ionic3 内容通过左右滑动手机屏幕切换大类/小类 获取数据刷新页面.
大小类列表:
html:
```

<div class="sort">
        <div class="sort-item">
            <div class="big" *ngFor="let v of category" (click)="getBigAds(v.uuid,v.name)" tappable [ngClass]="{active:selectedType==v.uuid }">
                {{v.name}}
            </div>
        </div>
    </div>

    <div class="sort" *ngIf="selectedType && subcategory?.length > 0">
        <div class="sort-item">
            <div class="small" *ngFor="let v of subtype" (click)="SmallList(v.uuid)" tappable [ngClass]="{active:selectedSubtype==v.uuid}">
                {{v.name}}
            </div>
        </div>
    </div>
<ion-content class="bg" (swipe)="doSwitch($event,selectedType,selectedSubtype)">
  内容(随uuid不同获取不同数据)
<ion-content>
```
ts:
```
doSwitch(e, categoryUuid, subCategoryUUid) {
    //滑动切换tag获取对应类型广告
    if (categoryUuid && !subCategoryUUid) {
      //大类切换
      for (var i = 0; i < this.category.length; i++) {
        if (this.category[i].uuid == categoryUuid) {
          if (i > 0 && Math.abs(e.deltaX) > 50 && e.deltaX > 0) {
            this.getBigAds(this.category[i - 1].uuid);
          }
          if (
            i < this.category.length &&
            Math.abs(e.deltaX) > 50 &&
            e.deltaX < 0
          ) {
            this.getBigAds(this.category[i + 1].uuid);
          }
        }
      }
    } else if (categoryUuid && subCategoryUUid) {
      //小类切换
      for (var i = 0; i < this.subCategory.length; i++) {
        if (this.subCategory[i].uuid == subCategoryUUid) {
          if (i > 0 && Math.abs(e.deltaX) > 50 && e.deltaX > 0) {
            this.SmallList(this.subCategory[i - 1].uuid);
          }
          if (
            i < this.subCategory.length &&
            Math.abs(e.deltaX) > 50 &&
            e.deltaX < 0
          ) {
            this.SmallList(this.subCategory[i + 1].uuid);
          }
        }
      }
    }
```
一.给ion-content 添加事件:(swipe)="doSwitch($event,selectedType,selectedSubtype)"该函数传入三个参数;
1.swipe事件对象,2.当前选中的状态的"大类uuid",2.当前选中的状态的"小类uuid"

二.判断if (categoryUuid && !subCategoryUUid){}
选中状态的是否大类小类两个都有选中状态,若只有大类选中则左右滑动获取的是大类的数据,大类小类都选中了则获取该小类的数据

三.要通过 e.deltaX 的正/负判断决定 i+1(前一个大/小类)或i减1(后一个大/小类)

四.this.getBigAds(大类的uuid)获取该大类对应的列表数据;同理this.SmallList(小类的uuid)获取该小类对应的列表数据;
