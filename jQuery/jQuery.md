### 带有index的循环

````
  1.清空数组
   _this.secondDepartmentList.splice(0, _this.secondDepartmentList.length);
  2.循环result中的值；后端传递的是一个List对象
  jQuery.each(result, function(idx, obj) {
      _this.secondDepartmentList.push({
          departId: obj.id,
          departName: obj.departName
      });
  });

```

### 前后端交互；Ajax请求向后台传递对象

```
1.前端传递
 var depart={
    'departId':departId,
    'departName':departName,
    'doctorId': this.doctorId,
    'conductType': type,
    'conductObjId':departId,
    'conductObjName':departName
};
jQuery.ajax({
    url: '/doctorDepart/addConductRecord',
    type:'POST',
    dataType: 'json',
    contentType: 'application/json',
    data:JSON.stringify(depart),
    success: function (result) {
        console.log(result);
    }
});
2.后端接收
 @RequestMapping("/addConductRecord")
 @ResponseBody
 public Result addConductRecord(@RequestBody ConductRecord conductRecord){
    return new DefaultResult<Boolean>(BusinessErrorCode.SUCCESS,true);
 }

```

### 按钮样式的切换

```
1.如果有pop-btn-ghost删除
if(jQuery("#"+id).hasClass("pop-btn-ghost")){
    jQuery("#"+id).removeClass("pop-btn-ghost");
}
2.删除id类似其他的pop-btn-primary
jQuery("#"+id).siblings().removeClass("pop-btn-primary");
3.给id添加pop-btn-primary样式；可以理解为给选中的按钮添加样式
jQuery("#"+id).addClass("pop-btn-primary");

```
