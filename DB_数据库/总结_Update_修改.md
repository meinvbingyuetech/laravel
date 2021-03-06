```php
<?php
# https://laravel.com/api/5.2/Illuminate/Database/Query/Builder.html
#use Illuminate\Support\Facades\DB;

# 获取表名
$table_name = $model->getTable();

app('db')->update( app('db')->raw($update_sql) );

$article = $model->find(1);
$article->title = $data['title'];
$article->save();

# 查询或创建
$model = app('ModelName');
$model = $model->firstOrCreate(['name' => $data['name']]);
$model->stock_num = $data['num'];
$model->last_edit_time = time();
$model->save();
        
$affected = DB::update('update users set votes = 100 where name = ?', ['John']);


#递增/递减
$article->increment('read_num');
$article->increment('read_num', 10);
$article->increment('detail_count', 1, [
        'use_count'=>DB::raw('use_count + 1'),
        'start_time' => date('YmdHis'),
])


$article->decrement('read_num');
$article->decrement('read_num', 10);

/**************************************************************************/
DB::table('users')
            ->where('id', 1)
            ->update(['votes' => 1]);

// 更新JSON字段
DB::table('users')
            ->where('id', 1)
            ->update([
                'options->enabled' => true,
                'visit_count' => DB::raw('visit_count + 1'),
            ]);
            
// 批量更新 
DB::table('as_novel_chapt')
            ->whereIn('id', $cid_arr)
            ->update(['flag' => 1]);
            
```
