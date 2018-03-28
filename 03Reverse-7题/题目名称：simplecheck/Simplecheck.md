### Simplecheck

- 手机安装apk后，是普通提交flag后台校验的程序，查了一下壳，没发现壳，直接反编译`class.dex`拿到jar包看到`MainActivity`：
```java
package com.a.simplecheck;

import android.content.Context;
import android.os.Bundle;
import android.support.v7.app.c;
import android.view.View;
import android.view.View.OnClickListener;
import android.widget.EditText;
import android.widget.Toast;

public class MainActivity
  extends c
{
  protected void onCreate(Bundle paramBundle)
  {
    super.onCreate(paramBundle);
    setContentView(2130968603);
    final EditText localEditText = (EditText)findViewById(2131427422);
    findViewById(2131427423).setOnClickListener(new View.OnClickListener()
    {
      public void onClick(View paramAnonymousView)
      {
        if (a.a(localEditText.getText().toString()))
        {
          Toast.makeText(jdField_this, "You get it~", 1).show();
          return;
        }
        Toast.makeText(jdField_this, "Sorry its wrong", 1).show();
      }
    });
  }
}
```

- 再跟进a方法，有：
```java
package com.a.simplecheck;

public class a
{
  private static int[] a = { 0, 146527998, 205327308, 94243885, 138810487, 408218567, 77866117, 71548549, 563255818, 559010506, 449018203, 576200653, 307283021, 467607947, 314806739, 341420795, 341420795, 469998524, 417733494, 342206934, 392460324, 382290309, 185532945, 364788505, 210058699, 198137551, 360748557, 440064477, 319861317, 676258995, 389214123, 829768461, 534844356, 427514172, 864054312 };
  private static int[] b = { 13710, 46393, 49151, 36900, 59564, 35883, 3517, 52957, 1509, 61207, 63274, 27694, 20932, 37997, 22069, 8438, 33995, 53298, 16908, 30902, 64602, 64028, 29629, 26537, 12026, 31610, 48639, 19968, 45654, 51972, 64956, 45293, 64752, 37108 };
  private static int[] c = { 38129, 57355, 22538, 47767, 8940, 4975, 27050, 56102, 21796, 41174, 63445, 53454, 28762, 59215, 16407, 64340, 37644, 59896, 41276, 25896, 27501, 38944, 37039, 38213, 61842, 43497, 9221, 9879, 14436, 60468, 19926, 47198, 8406, 64666 };
  private static int[] d = { 0, -341994984, -370404060, -257581614, -494024809, -135267265, 54930974, -155841406, 540422378, -107286502, -128056922, 265261633, 275964257, 119059597, 202392013, 283676377, 126284124, -68971076, 261217574, 197555158, -12893337, -10293675, 93868075, 121661845, 167461231, 123220255, 221507, 258914772, 180963987, 107841171, 41609001, 276531381, 169983906, 276158562 };
  
  public static boolean a(String paramString)
  {
    if (paramString.length() != b.length) {
      return false;
    }
    int[] arrayOfInt = new int[a.length];
    arrayOfInt[0] = 0;
    byte[] arrayOfByte = paramString.getBytes();
    int i = arrayOfByte.length;
    int j = 0;
    int k = 1;
    while (j < i)
    {
      arrayOfInt[k] = arrayOfByte[j];
      k++;
      j++;
    }
    for (int m = 0;; m++)
    {
      if (m >= c.length) {
        break label175;
      }
      if ((a[m] != b[m] * arrayOfInt[m] * arrayOfInt[m] + c[m] * arrayOfInt[m] + d[m]) || (a[(m + 1)] != b[m] * arrayOfInt[(m + 1)] * arrayOfInt[(m + 1)] + c[m] * arrayOfInt[(m + 1)] + d[m])) {
        break;
      }
    }
    label175:
    return true;
  }
}
```

- 根据上面的`a`类里的验证算法，可以写出如下脚本：
```python
#!/usr/bin/python3
a = [ 0, 146527998, 205327308, 94243885, 138810487, 408218567, 77866117, 71548549, 563255818, 559010506, 449018203, 576200653, 307283021, 467607947, 314806739, 341420795, 341420795, 469998524, 417733494, 342206934, 392460324, 382290309, 185532945, 364788505, 210058699, 198137551, 360748557, 440064477, 319861317, 676258995, 389214123, 829768461, 534844356, 427514172, 864054312 ]
b = [ 13710, 46393, 49151, 36900, 59564, 35883, 3517, 52957, 1509, 61207, 63274, 27694, 20932, 37997, 22069, 8438, 33995, 53298, 16908, 30902, 64602, 64028, 29629, 26537, 12026, 31610, 48639, 19968, 45654, 51972, 64956, 45293, 64752, 37108 ]
c = [ 38129, 57355, 22538, 47767, 8940, 4975, 27050, 56102, 21796, 41174, 63445, 53454, 28762, 59215, 16407, 64340, 37644, 59896, 41276, 25896, 27501, 38944, 37039, 38213, 61842, 43497, 9221, 9879, 14436, 60468, 19926, 47198, 8406, 64666 ]
d = [ 0, -341994984, -370404060, -257581614, -494024809, -135267265, 54930974, -155841406, 540422378, -107286502, -128056922, 265261633, 275964257, 119059597, 202392013, 283676377, 126284124, -68971076, 261217574, 197555158, -12893337, -10293675, 93868075, 121661845, 167461231, 123220255, 221507, 258914772, 180963987, 107841171, 41609001, 276531381, 169983906, 276158562 ]
e = "0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLNMOPQRSTUVWXYZ!#$%&()*+-/<>=@?_{}"

arrayOfInt = [0 for i in range(len(c))]

for m in range(len(c)):
    for n in range (len(e)):
        arrayOfInt[m] = ord(e[n])
        if (a[m] == b[m] * arrayOfInt[m] * arrayOfInt[m] + c[m] * arrayOfInt[m] + d[m]):
            flag += e[n]
print(flag+'}')
```

- 获得flag为：`flag{MAth_i&_GOOd_DON7_90V_7hInK?}`
