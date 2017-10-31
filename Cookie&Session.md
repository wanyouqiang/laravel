# Cookie&Session
### Laravel 与 Cookie
* 获取Cookie值 $request->cookie('key');
* 设置Cookie return response('abc')->cooke('key', 'value');

### Laravel 与 Session
* 设置Session $request->session()->put('key', 'value');
* 获取Session $request->session()->get('key');
* 删除Session $request->session()->forget('key');