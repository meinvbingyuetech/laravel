### 创建服务 app\Providers\ComposerServiceProvider.php

```php

namespace App\Providers;

use App\Http\ViewComposers\AppComposer;
use App\Http\ViewComposers\InformationComposer;
use Illuminate\Support\ServiceProvider;

class ComposerServiceProvider extends ServiceProvider
{
    /**
     * 在容器中注册绑定.
     *
     * @return void
     */
    public function boot()
    {
        // 非命令行模式下才执行，防止预渲染模板并压缩模板时报错
        if (!\App::runningInConsole()) {
            view()->composers([
                InformationComposer::class      => [
                    'web.information.*'
                ],
                AppComposer::class => [
                    'web.common.phone_app',
                    'web.common.right_shortcuts',
                ],
            ]);
            
            // 单个绑定
            view()->composer('article.list', 'App\Http\ViewComposers\CategoriesComposer');
        }
    }

    /**
     * Register any application services.
     *
     * @return void
     */
    public function register()
    {

    }
}

```

### 注册服务

```
\config\app.php

'providers' => [
    \App\Providers\ComposerServiceProvider::class,
]

```
 
### 创建composer
```php
namespace App\Http\ViewComposers;

use Illuminate\View\View;

class AppComposer
{
    /**
     * 绑定数据到视图.
     *
     * @param View $view
     * @return void
     */
    public function compose(View $view)
    {
        $data = [
            'android_link' => config('download.app.android'),
            'ios_link' => config('download.app.ios'),
            'qq_app_link'   =>  config('download.app.qq'),
        ];

        foreach($data as $ak=>$av){
            $view->with($ak,$av);
        }

        return $view;
    }
}
```

### 模板中使用
```php

<h1>{{ $qq_app_link }}</h1>

```
