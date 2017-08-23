/**
 * 缓存管理
 * @param mixed     $name 缓存名称，如果为数组表示进行缓存设置
 * @param mixed     $value 缓存值
 * @param mixed     $options 缓存参数
 * @param string    $tag 缓存标签
 * @return mixed
 */
function cache($name, $value = '', $options = null, $tag = null)
{
    define('CACHE_PATH', S_ROOT . 'cache');

    require S_ROOT . 'vendor/autoload.php';
    if (is_array($options)) {
        // 缓存操作的同时初始化
        \king192\phpcache\Cache::connect($options);
    } elseif (is_array($name)) {
        // 缓存初始化
        return \king192\phpcache\Cache::connect($name);
    }
    if (is_null($name)) {
        return \king192\phpcache\Cache::clear($value);
    } elseif ('' === $value) {
        // 获取缓存
        return 0 === strpos($name, '?') ? \king192\phpcache\Cache::has(substr($name, 1)) : \king192\phpcache\Cache::get($name);
    } elseif (is_null($value)) {
        // 删除缓存
        return \king192\phpcache\Cache::rm($name);
    } elseif (0 === strpos($name, '?') && '' !== $value) {
        $expire = is_numeric($options) ? $options : null;
        return \king192\phpcache\Cache::remember(substr($name, 1), $value, $expire);
    } else {
        // 缓存数据
        if (is_array($options)) {
            $expire = isset($options['expire']) ? $options['expire'] : null; //修复查询缓存无法设置过期时间
        } else {
            $expire = is_numeric($options) ? $options : null; //默认快捷缓存设置过期时间
        }
        if (is_null($tag)) {
            return \king192\phpcache\Cache::set($name, $value, $expire);
        } else {
            return \king192\phpcache\Cache::tag($tag)->set($name, $value, $expire);
        }
    }
}