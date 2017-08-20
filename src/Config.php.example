<?php

namespace king192\phpcache;

class Config {
	public static $cache = array(
        // 驱动方式
        'type'   => 'File',
        // 缓存保存目录
        'path'   => CACHE_PATH,
        // 缓存前缀
        'prefix' => '',
        // 缓存有效期 0表示永久缓存
        'expire' => 0,
    );

    public static function get($name) {
    	$arr_data self::$cache;
    	if (-1 != strpos('.', $name)) {
    		$arr_kv = explode('.', $name);
    		return $arr_data[$arr_kv[1]];
    	}
    	if ('cache' !== $name) {
    		throw new Exception("Error Processing Request");
    	}
    	return $arr_data;
    }
}