# NotePad
添加时间戳功能：
1.添加时间戳的位置在主页面的每个列表项中添加，即在notelist_item.xml布局文件中添加一个

   <TextView  
       android:id="@android:id/text2"  
       android:layout_width="match_parent"  
       android:layout_height="wrap_content"  
       android:textAppearance="?android:attr/textAppearanceLarge"  
       android:gravity="center_vertical"  
       android:paddingLeft="5dp"  
       android:singleLine="true" />  
2.在NoteList类的PROJECTION中添加COLUMN_NAME_MODIFICATION_DATE字段(该字段在NotePad中有说明)

  // The columns needed by the cursor adapter
  private static final String[] PROJECTION = new String[] {    
      NotePad.Notes._ID, // 0    
      NotePad.Notes.COLUMN_NAME_TITLE, // 1    
      NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE,//在这里加入了修改时间的显示    
   };    

3.修改适配器内容，增加dataColumns中装配到ListView的内容，所以要同时增加一个ID标识来存放该时间参数。

  // The names of the cursor columns to display in the view, initialized to the title column
  String[] dataColumns = { NotePad.Notes.COLUMN_NAME_TITLE,
       NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE //增加时间参数} ;
  // The view IDs that will display the cursor columns, initialized to the TextView in noteslist_item.xml
  int[] viewIDs = { android.R.id.text1 ,android.R.id.text2};


4.在NoteEditor文件的updateNote方法中获取当前系统的时间，并对时间进行格式化

   // Sets up a map to contain values to be updated in the provider.   
      ContentValues values = new ContentValues();  
   // 转化时间格式
      Long now = Long.valueOf(System.currentTimeMillis());  
      SimpleDateFormat sf = new SimpleDateFormat("yy/MM/dd HH:mm");  
      Date d = new Date(now);  
      String format = sf.format(d);  
      values.put(NotePad.Notes.COLUMN_NAME_MODIFICATION_DATE, format);
结果图片展示
![Image text](https://img-blog.csdnimg.cn/20210421010006288.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dvbmdtYW5Db2Rpbmc=,size_16,color_FFFFFF,t_0)
![Image text](https://img-blog.csdnimg.cn/20210421010006324.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dvbmdtYW5Db2Rpbmc=,size_16,color_FFFFFF,t_0)
