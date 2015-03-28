# Debug Write

### Display the names and values of all variables in the local variable environment, including private variables.

---

#### FEATURES

* Uses an argumentless [`zwrite`](http://docs.intersystems.com/cache20091/csp/docbook/DocBook.UI.Page.cls?KEY=RCOS_czwrite) under the hood.
	* Process-private globals are not displayed.
	* Variables are listed by name in ASCII order.
	* Subscripted variables are listed in subscript tree order.
* The debug output's `HTML` has been escaped.
* Debug output labels (optional).
* User-defined URI query string (optional).
* Unlimited `<pre>` tag `HTML` attributes.

----

#### USAGE

##### Tags:

Output debug info into the document right before the `<!doctype html>` tag:

```html
<custom:rg:debug:write prehtml />
```

Output debug info into the `<head>` right before the closing `</head>` tag:

```html
<custom:rg:debug:write head />
```

Output debug info into the `<body>` right before the closing `</body>` tag:

```html
<custom:rg:debug:write body />
```

Output debug info into the document right after the closing `</html>` tag:

```html
<custom:rg:debug:write posthtml />
```

Output debug info into the current location:

```html
<custom:rg:debug:write />
```

… and/or `this`:

```html
<custom:rg:debug:write this />
```

Output debug info into all of the above sections:

```html
<custom:rg:debug:write this prehtml head body posthtml />
```

##### API:

Output debug info without `querystring` and `label` arguments (the empty strings can be omitted - keep the commas though):

```
do ##class(custom.rg.debug.Write).all("", "", "class|pretty")
```

---

#### ATTRIBUTES

1. `prehtml`: `PREHTML` section (before `<!doctype html>`).
2. `head`: `HEAD` section (in `<head>`).
3. `body`: `BODY` section (in `<body>`).
4. `posthtml`: `POSTHTML` section (after `</html>`).
5. `this`: Called in place (immediately invoked).
6. `attributes`: Delimited attributes to include in the `<pre>` tag. Format: `attribute|value, attribute|value, …`.
7. `querystring`: Query string parameter to use. Default is `debug`.

The `prehtml`, `head`, `body` and `posthtml` attributes accept a numeric value which is used to relatively position the content within the section; a negative number is the beginning of a section and a positive number is the end of a section. The default value is `1`.

**Note:** The `this` attribute doesn't (currently) take any arguments.

---

#### EXAMPLES

**Before:**

```html
<!doctype html>
<html>
<head>
<meta charset="utf-8">
<title>Testing Debug Write</title>
<meta name="description" content="">
<meta name="keywords" content="">
</head>
<body>
<custom:rg:debug:write this prehtml head body="2" posthtml attributes="class|pretty" />
<p>Nam ut aliquet diam. Suspendisse et mauris tempus urna ullamcorper placerat at non eros. Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vivamus pharetra pellentesque sem eu dictum. Nunc accumsan sagittis velit, quis condimentum diam hendrerit a. Fusce sollicitudin, nibh eu bibendum aliquet, magna nulla lacinia nisl, vitae imperdiet diam leo quis massa. Pellentesque sodales rutrum tempus. Vivamus nunc nisi, auctor bibendum condimentum quis, tincidunt sed magna. Nunc a quam dolor. Donec eu mi in diam placerat luctus in in massa. Etiam commodo, odio et lobortis condimentum, sapien sapien iaculis tellus, et rhoncus leo massa ut diam. Ut molestie aliquet lectus, nec condimentum sem mollis id.</p>
#[ do ##class(custom.rg.debug.Write).all("", "right here!", "class|pretty") ]#
<p>Praesent sit amet nulla tortor. Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas. Proin elementum tincidunt arcu at congue. Sed rhoncus suscipit augue, in placerat eros vestibulum nec. Donec ac diam turpis, sed lobortis ipsum. Ut eros est, laoreet ac hendrerit id, ullamcorper id lorem. Aenean fermentum lectus eu purus gravida facilisis eu at elit. Donec sapien justo, sollicitudin eget commodo in, blandit non massa. Nulla quis sagittis velit. Phasellus nibh diam, viverra consequat tincidunt et, luctus eget nunc. Cras aliquet lacinia dolor vel condimentum.</p>
</body>
</html>
```

**After:**

```html
<pre class="pretty">
---------- PREHTML ----------
%CSPsc=1
%request=&lt;OBJECT REFERENCE&gt;[1@%CSP.Request]
%response=&lt;OBJECT REFERENCE&gt;[2@%CSP.Response]
%session=&lt;OBJECT REFERENCE&gt;[3@%CSP.Session]
&lt;Private variables&gt;
qs=&quot;debug&quot;
label=&quot;prehtml&quot;
attr=&quot;class|pretty&quot;
currIO=&quot;UTF8&quot;
---------- PREHTML ----------
</pre>
<!doctype html>
<html>
<head>
<meta charset="utf-8">
<title>Testing Debug Write</title>
<meta name="description" content="">
<meta name="keywords" content="">
<pre class="pretty">
---------- HEAD ----------
%CSPsc=1
%request=&lt;OBJECT REFERENCE&gt;[1@%CSP.Request]
%response=&lt;OBJECT REFERENCE&gt;[2@%CSP.Response]
%session=&lt;OBJECT REFERENCE&gt;[3@%CSP.Session]
&lt;Private variables&gt;
qs=&quot;debug&quot;
label=&quot;head&quot;
attr=&quot;class|pretty&quot;
currIO=&quot;UTF8&quot;
---------- HEAD ----------
</pre>
</head>
<body>
<a class="drs" href="#">&nbsp;<span><b>&lt;custom:rg:debug:write this="" prehtml="" head="" body="2" posthtml="" attributes="class|pretty"&gt;</b></span></a>
<pre class="pretty">
---------- THIS ----------
%CSPsc=1
%request=&lt;OBJECT REFERENCE&gt;[1@%CSP.Request]
%response=&lt;OBJECT REFERENCE&gt;[2@%CSP.Response]
%session=&lt;OBJECT REFERENCE&gt;[3@%CSP.Session]
Timers=1
Timers(1)=&quot;	����9U�c&amp;��&quot;
Timers(1,&quot;Attributes&quot;)=&quot;this=&quot;&quot; prehtml=&quot;&quot; head=&quot;&quot; body=&quot;2&quot; posthtml=&quot;&quot; attributes=&quot;class|pretty&quot;&quot;
&lt;Private variables&gt;
qs=&quot;debug&quot;
label=&quot;this&quot;
attr=&quot;class|pretty&quot;
currIO=&quot;UTF8&quot;
---------- THIS ----------
</pre>
<a class="dre" href="#">&nbsp;<span><b>&lt;/custom:rg:debug:write this="" prehtml="" head="" body="2" posthtml="" attributes="class|pretty"&gt;</b><br>Elapsed: .000475<br>Globals: 3<br>Lines: 51</span></a>
<p>Nam ut aliquet diam. Suspendisse et mauris tempus urna ullamcorper placerat at non eros. Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vivamus pharetra pellentesque sem eu dictum. Nunc accumsan sagittis velit, quis condimentum diam hendrerit a. Fusce sollicitudin, nibh eu bibendum aliquet, magna nulla lacinia nisl, vitae imperdiet diam leo quis massa. Pellentesque sodales rutrum tempus. Vivamus nunc nisi, auctor bibendum condimentum quis, tincidunt sed magna. Nunc a quam dolor. Donec eu mi in diam placerat luctus in in massa. Etiam commodo, odio et lobortis condimentum, sapien sapien iaculis tellus, et rhoncus leo massa ut diam. Ut molestie aliquet lectus, nec condimentum sem mollis id.</p>
<pre class="pretty">
---------- RIGHT HERE! ----------
%CSPsc=1
%mmmu1=&quot;&quot;
%mmmu2=&quot;&quot;
%request=&lt;OBJECT REFERENCE&gt;[1@%CSP.Request]
%response=&lt;OBJECT REFERENCE&gt;[2@%CSP.Response]
%session=&lt;OBJECT REFERENCE&gt;[3@%CSP.Session]
Timers=0
gRequestId=82529275
ruleError=&quot;&quot;
&lt;Private variables&gt;
qs=&quot;&quot;
label=&quot;right here!&quot;
attr=&quot;class|pretty&quot;
currIO=&quot;UTF8&quot;
---------- RIGHT HERE! ----------
</pre>
<p>Praesent sit amet nulla tortor. Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas. Proin elementum tincidunt arcu at congue. Sed rhoncus suscipit augue, in placerat eros vestibulum nec. Donec ac diam turpis, sed lobortis ipsum. Ut eros est, laoreet ac hendrerit id, ullamcorper id lorem. Aenean fermentum lectus eu purus gravida facilisis eu at elit. Donec sapien justo, sollicitudin eget commodo in, blandit non massa. Nulla quis sagittis velit. Phasellus nibh diam, viverra consequat tincidunt et, luctus eget nunc. Cras aliquet lacinia dolor vel condimentum.</p>
<pre class="pretty">
---------- BODY ----------
%CSPsc=1
%mmmu1=&quot;&quot;
%mmmu2=&quot;&quot;
%request=&lt;OBJECT REFERENCE&gt;[1@%CSP.Request]
%response=&lt;OBJECT REFERENCE&gt;[2@%CSP.Response]
%session=&lt;OBJECT REFERENCE&gt;[3@%CSP.Session]
Timers=0
gRequestId=82529275
ruleError=&quot;&quot;
&lt;Private variables&gt;
qs=&quot;debug&quot;
label=&quot;body&quot;
attr=&quot;class|pretty&quot;
currIO=&quot;UTF8&quot;
---------- BODY ----------
</pre>
</body>
</html>
<pre class="pretty">
---------- POSTHTML ----------
%CSPsc=1
%mmmu1=&quot;&quot;
%mmmu2=&quot;&quot;
%request=&lt;OBJECT REFERENCE&gt;[1@%CSP.Request]
%response=&lt;OBJECT REFERENCE&gt;[2@%CSP.Response]
%session=&lt;OBJECT REFERENCE&gt;[3@%CSP.Session]
Timers=0
gRequestId=82529275
ruleError=&quot;&quot;
&lt;Private variables&gt;
qs=&quot;debug&quot;
label=&quot;posthtml&quot;
attr=&quot;class|pretty&quot;
currIO=&quot;UTF8&quot;
---------- POSTHTML ----------
</pre>
```

… and, here's the HTML output:

> <pre class="pretty">
> ---------- PREHTML ----------
> %CSPsc=1
> %request=&lt;OBJECT REFERENCE&gt;[1@%CSP.Request]
> %response=&lt;OBJECT REFERENCE&gt;[2@%CSP.Response]
> %session=&lt;OBJECT REFERENCE&gt;[3@%CSP.Session]
> &lt;Private variables&gt;
> qs=&quot;debug&quot;
> label=&quot;prehtml&quot;
> attr=&quot;class|pretty&quot;
> currIO=&quot;UTF8&quot;
> ---------- PREHTML ----------
> </pre>
> <pre class="pretty">
> ---------- HEAD ----------
> %CSPsc=1
> %request=&lt;OBJECT REFERENCE&gt;[1@%CSP.Request]
> %response=&lt;OBJECT REFERENCE&gt;[2@%CSP.Response]
> %session=&lt;OBJECT REFERENCE&gt;[3@%CSP.Session]
> &lt;Private variables&gt;
> qs=&quot;debug&quot;
> label=&quot;head&quot;
> attr=&quot;class|pretty&quot;
> currIO=&quot;UTF8&quot;
> ---------- HEAD ----------
> </pre>
> <pre class="pretty">
> ---------- THIS ----------
> %CSPsc=1
> %request=&lt;OBJECT REFERENCE&gt;[1@%CSP.Request]
> %response=&lt;OBJECT REFERENCE&gt;[2@%CSP.Response]
> %session=&lt;OBJECT REFERENCE&gt;[3@%CSP.Session]
> Timers=1
> Timers(1)=&quot;	����9U�c&amp;��&quot;
> Timers(1,&quot;Attributes&quot;)=&quot;this=&quot;&quot; prehtml=&quot;&quot; head=&quot;&quot; body=&quot;2&quot; posthtml=&quot;&quot; attributes=&quot;class|pretty&quot;&quot;
> &lt;Private variables&gt;
> qs=&quot;debug&quot;
> label=&quot;this&quot;
> attr=&quot;class|pretty&quot;
> currIO=&quot;UTF8&quot;
> ---------- THIS ----------
> </pre>
> <p>Nam ut aliquet diam. Suspendisse et mauris tempus urna ullamcorper placerat at non eros. Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vivamus pharetra pellentesque sem eu dictum. Nunc accumsan sagittis velit, quis condimentum diam hendrerit a. Fusce sollicitudin, nibh eu bibendum aliquet, magna nulla lacinia nisl, vitae imperdiet diam leo quis massa. Pellentesque sodales rutrum tempus. Vivamus nunc nisi, auctor bibendum condimentum quis, tincidunt sed magna. Nunc a quam dolor. Donec eu mi in diam placerat luctus in in massa. Etiam commodo, odio et lobortis condimentum, sapien sapien iaculis tellus, et rhoncus leo massa ut diam. Ut molestie aliquet lectus, nec condimentum sem mollis id.</p>
> <pre class="pretty">
> ---------- RIGHT HERE! ----------
> %CSPsc=1
> %mmmu1=&quot;&quot;
> %mmmu2=&quot;&quot;
> %request=&lt;OBJECT REFERENCE&gt;[1@%CSP.Request]
> %response=&lt;OBJECT REFERENCE&gt;[2@%CSP.Response]
> %session=&lt;OBJECT REFERENCE&gt;[3@%CSP.Session]
> Timers=0
> gRequestId=82529275
> ruleError=&quot;&quot;
> &lt;Private variables&gt;
> qs=&quot;&quot;
> label=&quot;right here!&quot;
> attr=&quot;class|pretty&quot;
> currIO=&quot;UTF8&quot;
> ---------- RIGHT HERE! ----------
> </pre>
> <p>Praesent sit amet nulla tortor. Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas. Proin elementum tincidunt arcu at congue. Sed rhoncus suscipit augue, in placerat eros vestibulum nec. Donec ac diam turpis, sed lobortis ipsum. Ut eros est, laoreet ac hendrerit id, ullamcorper id lorem. Aenean fermentum lectus eu purus gravida facilisis eu at elit. Donec sapien justo, sollicitudin eget commodo in, blandit non massa. Nulla quis sagittis velit. Phasellus nibh diam, viverra consequat tincidunt et, luctus eget nunc. Cras aliquet lacinia dolor vel condimentum.</p>
> <pre class="pretty">
> ---------- BODY ----------
> %CSPsc=1
> %mmmu1=&quot;&quot;
> %mmmu2=&quot;&quot;
> %request=&lt;OBJECT REFERENCE&gt;[1@%CSP.Request]
> %response=&lt;OBJECT REFERENCE&gt;[2@%CSP.Response]
> %session=&lt;OBJECT REFERENCE&gt;[3@%CSP.Session]
> Timers=0
> gRequestId=82529275
> ruleError=&quot;&quot;
> &lt;Private variables&gt;
> qs=&quot;debug&quot;
> label=&quot;body&quot;
> attr=&quot;class|pretty&quot;
> currIO=&quot;UTF8&quot;
> ---------- BODY ----------
> </pre>
> <pre class="pretty">
> ---------- POSTHTML ----------
> %CSPsc=1
> %mmmu1=&quot;&quot;
> %mmmu2=&quot;&quot;
> %request=&lt;OBJECT REFERENCE&gt;[1@%CSP.Request]
> %response=&lt;OBJECT REFERENCE&gt;[2@%CSP.Response]
> %session=&lt;OBJECT REFERENCE&gt;[3@%CSP.Session]
> Timers=0
> gRequestId=82529275
> ruleError=&quot;&quot;
> &lt;Private variables&gt;
> qs=&quot;debug&quot;
> label=&quot;posthtml&quot;
> attr=&quot;class|pretty&quot;
> currIO=&quot;UTF8&quot;
> ---------- POSTHTML ----------
> </pre>

**See also:** [`test.csp`](https://github.com/mhulse/csp-debug-write/blob/master/write/test.csp).

---

#### INSTALLATION

There's a couple ways (that I can think of) to install this code:

### Copy/paste:

1. Open Studio.
2. Change to the `CMS` namespace.
3. "File" >> "New..." and choose "Caché Class Definition" from "General" tab.
4. Copy/paste the **RAW** contents `custom.rg.debug.Write.cls` into this new file.
5. Save this file as `custom.rg.debug.Write.cls` to `custom/rg/debug` package.
6. Compile.
7. "File" >> "New..." and choose "Caché Server Page" from "CSP File" tab.
8. Copy/paste the **RAW** contents of `custom.rg.debug.WriteRule.csr` into this new file.
9. Save this file as `custom.rg.debug.WriteRule.csr` to the "CSP Files" >> `/csp/cms/customrules` package/folder/location.
10. Compile.

### Import local:

1. Download and unzip this repo to your local machine.
2. Open Studio.
3. Change to the `CMS` namespace.
4. "Tools" >> "Import Local...".
5. Import `custom.rg.debug.Write.xml`, `custom.rg.debug.WriteRule.csr` and check the compile box.

---

#### NOTES:

Non-[DTI](http://www.dtint.com/) customers should emove these lines from `custom.rg.debug.WriteRule.csr`:

```
<csr:class super="dt.common.page.Rule" />
```

```
<script language="cache" runat="compiler">
	do ##this.RenderDTStartTag()
</script>
```

```
<script language="cache" runat="compiler">
	do ##this.RenderDTEndTag()
</script>
```

---

#### DISCUSSION(S)

* [Applying ..EscapeHTML to argumentless write OR zwrite?](https://groups.google.com/d/topic/intersystems-public-cache/-CmAIWmtHdY/discussion)

---

#### LEGAL

Copyright © 2012 [Micky Hulse](http://mky.io)

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this work except in compliance with the License. You may obtain a copy of the License in the LICENSE file, or at:

[http://www.apache.org/licenses/LICENSE-2.0](http://www.apache.org/licenses/LICENSE-2.0)

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.
