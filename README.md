# Android开发实验三
### 1、Android ListView的用法
##### 思路

①在layout文件中编写组件，用SimpleAdapter绑定组件，再循环输出
②选中列表项时，用Toast显示选中列表项的name值

##### 部分实验代码
```
public class MainActivity extends AppCompatActivity {
    private String[] names = new String[]
            {"Lion","Tiger","Monkey","Dog","Cat","Elephant"};
   	 private int[] imgIds = new int[]
            {R.drawable.lion,R.drawable.tiger,R.drawable.monkey,R.drawable.dog,R.drawable.cat,R.drawable.elephant};

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        List<Map<String,Object>> lists = new ArrayList<>();
        for (int i = 0;i < names.length;i++){
            Map<String,Object> list = new HashMap<>();
            list.put("images",imgIds[i]);
            list.put("names",names[i]);
            lists.add(list);
        }

        ListView listView = findViewById(R.id.list_view01);

        SimpleAdapter simpleAdapter = new SimpleAdapter(MainActivity.this,
                lists, R.layout.simple_adapter,
                new String[] {"names","images"},
                new int[] {R.id.names,R.id.images});
        listView.setAdapter(simpleAdapter);

        listView.setOnItemClickListener(new AdapterView.OnItemClickListener(){

            @Override
            public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
                Toast toast = Toast.makeText(MainActivity.this,names[position],Toast.LENGTH_SHORT);
                toast.show();
            }
        });

    }
}
```
##### 实验结果

![Test1.png](./image/Test1.png)

### 2、创建自定义布局的AlertDialog
##### 思路

①通过AlertDialog.builder设置builder.setView

##### 部分实验代码
```
class AlertDialog extends Dialog {
    public AlertDialog(@NonNull Context context) {
        super(context);
    }
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        setContentView(R.layout.activity_main2);

        WindowManager windowManager = getWindow().getWindowManager();
        Display display = windowManager.getDefaultDisplay();
        WindowManager.LayoutParams p = getWindow().getAttributes();
        Point size = new Point();
        display.getSize(size);
        p.width = size.x;
        getWindow().setAttributes(p);
    }
}
```
##### 实验结果

![Test2.png](./image/Test2.png)

### 3、使用XML定义菜单
##### 思路

①在MenuActivity中装填对应的菜单并添加到menu中

##### 部分实验代码
```
@Override
    public boolean onCreateOptionsMenu(Menu menu) {
        MenuInflater menuinflater = getMenuInflater();
        menuinflater.inflate(R.menu.menu, menu);
        return true;
    }

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        switch (item.getItemId()) {
            case R.id.menu_size_1:
                test.setTextSize(10);
                return true;
            case R.id.menu_size_2:
                test.setTextSize(16);
                return true;
            case R.id.menu_size_3:
                test.setTextSize(20);
                return true;
            case R.id.menu_common:
                Toast.makeText(MainActivity.this,"这是普通菜单项",Toast.LENGTH_SHORT).show();
                return true;
            case R.id.menu_color_red:
                test.setTextColor(Color.RED);
                return true;
            case R.id.menu_color_black:
                test.setTextColor(Color.BLACK);
                return true;
            default:
                return super.onOptionsItemSelected(item);
        }
    }

```
##### 实验结果

![Test3.1.png](./image/Test3.1.png)
![Test3.2.png](./image/Test3.2.png)
![Test3.3.png](./image/Test3.3.png)
![Test3.4.png](./image/Test3.4.png)

### 3、创建上下文操作模式(ActionMode)的上下文菜单
##### 思路

①ActionModeActivity中注册View，实现ActionMode.callback回调

##### 部分实验代码
```
public void click_select(View V)
    {
        if (am == null)
            am = startActionMode(callback);

        LinearLayout line_lay = (LinearLayout) V;
        if (vis.get(V) == null || !vis.get(V))
        {
            line_lay.setBackgroundColor(Color.CYAN);
            vis.put(V, true);
            selected_items++;
        }
        else
        {
            line_lay.setBackgroundColor(Color.WHITE);
            vis.put(V, false);
            selected_items--;
        }
        callback.onActionItemClicked(am, null);

    }

    ActionMode.Callback callback = new ActionMode.Callback()
    {

        @Override
        public boolean onCreateActionMode(ActionMode actionMode, Menu menu)
        {
            getMenuInflater().inflate(R.menu.menu, menu);
            return true;
        }

        @Override
        public boolean onPrepareActionMode(ActionMode actionMode, Menu menu)
        {
            return false;
        }

        @Override
        public boolean onActionItemClicked(ActionMode actionMode, MenuItem menuItem)
        {
            actionMode.setTitle(selected_items + " selected");
            return true;
        }

        @Override
        public void onDestroyActionMode(ActionMode actionMode)
        {

        }
    };
```
##### 实验结果

![Test4.png](./image/Test4.png)
![Test4.1.png](./image/Test4.1.png)











