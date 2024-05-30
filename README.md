### Description:
<br>
The challenge is to write an application to convert colors from one model to another. The application supports the following color models: RGB, LAB, XYZ, HSV, HSL, CMYK. The application also warns the user about the possibility of color loss during some translations from one color model to another. To implement the application, a programming language С# and framework WPF were chosen.The application works according to the principle of converting all color models to RGB, and then to the one that is needed if the RGB system is not required.

For translations from one color model to another, the following formulas were used:

#### From RGB to CMYK

![](https://raw.githubusercontent.com/NikitaBeliayev/BSU-Computer-Graphic-Programming/development/Color%20Models/.media/img_8.png)

#### From RGB to XYZ: 

![](https://raw.githubusercontent.com/NikitaBeliayev/BSU-Computer-Graphic-Programming/development/Color%20Models/.media/img_20.png)

#### From XYZ to LAB:

![](https://github.com/NikitaBeliayev/BSU-Computer-Graphic-Programming/blob/development/Color%20Models/.media/img_30.png)


#### From XYZ to LUV:

![](https://raw.githubusercontent.com/NikitaBeliayev/BSU-Computer-Graphic-Programming/development/Color%20Models/.media/img_39.png)

### Code examples:
<br>
Method to convert from XYZ system to RGB:
<br>

``` cs
private System.Windows.Media.Color XyzToRgb(string[] mas)
{
    ColorHelper.XYZ xyz = new ColorHelper.XYZ(int.Parse(mas[0]), 
    int.Parse(mas[1]), int.Parse(mas[2]));
    ColorHelper.RGB rgb = ColorHelper.ColorConverter.XyzToRgb(xyz);
    System.Windows.Media.Color color = new System.Windows.Media.Color();
    color.R = rgb.R;
    color.G = rgb.G;
    color.B = rgb.B;
    return color;
}
```
<br>
Method to convert from  LAB system to RGB:
<br>

``` cs
private System.Windows.Media.Color LabToRgb(string[] mas)
{
    double L = double.Parse(mas[0]);
    double A = double.Parse(mas[1]);
    double B = double.Parse(mas[2])
    double X, Y, Z, x, y, z, f_x, f_y, f_z
    f_y = (L + 16.0) / 116.0;
    f_x = (A / 500.0) + f_y;
    f_z = f_y - (B / 200.0)
    if (Math.Pow(f_x, 3) > 0.008856)
    {
        x = Math.Pow(f_x, 3);
    }
    else
    {
        x = ((116.0 * f_x) - 16.0) / 903.3;
    }

    if (L > 903.3 * 0.008856)
    {
        y = Math.Pow(((L + 16.0) / 116.0), 3);
    }
    else
    {
        y = L / 903.3;
    }
    if (Math.Pow(f_z, 3) > 0.008856)
    {
        z = Math.Pow(f_z, 3);
    }
    else
    {
        z = ((116.0 * f_z) - 16.0) / 903.3;
    }
    X = x * 95.047;
    Y = y * 100.0;
    Z = z * 108.883
    XYZ xyz = new XYZ(X, Y, Z);
    RGB rgb = ColorHelper.ColorConverter.XyzToRgb(xyz)
    System.Windows.Media.Color color = new System.Windows.Media.Color();
    color.R = rgb.R;
    color.G = rgb.G;
    color.B = rgb.B;
    return color
}
```

<br>
Method to convert from HSL system to RGB:
<br>

``` cs
private System.Windows.Media.Color HslToRgb(string[] mas)
{
    ColorHelper.HSL hsl = new ColorHelper.HSL(int.Parse(mas[0]), byte.Parse(mas[1]), byte.Parse(mas[2]));
    ColorHelper.RGB rgb = ColorHelper.ColorConverter.HslToRgb(hsl);
    System.Windows.Media.Color color = new System.Windows.Media.Color();
    color.R = rgb.R;
    color.G = rgb.G;
    color.B = rgb.B;
    return color;
}
```
<br>
<br>
To avoid the use of a large number of conditional structures, dictionaries were used. The key is a string that indicates which system to translate into, and the value is an action type delegate that points to the appropriate method for translating:
<br>

``` cs
private Dictionary<string, Action<System.Windows.Media.Color>> from_rgb_to_another;
from_rgb_to_another = new Dictionary<string, Action<System.Windows.Media.Color>>()
{
    {"RGB", ToRGB },
    {"HSL", ToHSL },
    {"HSV", ToHSV },
    {"CMYK", ToCMYK },
    {"LAB", ToLAB },
    {"XYZ", ToXYZ }
};
```
<br>
Also, to check the correctness of the entered values, the corresponding methods for checking are provided:
<br>

``` cs
private bool CheckHex(string hex)
{
    return long.TryParse(hex, System.Globalization.NumberStyles.HexNumber, null, out long _);
}
private bool CheckRgb(string[] mas)
{
    bool result = byte.TryParse(mas[0], out byte _) && byte.TryParse(mas[1], out _) && byte.TryParse(mas[2], out _);
    return result;
}
private bool CheckHsv(string[] mas)
{
    bool first = short.TryParse(mas[0], out short H);
    bool second = byte.TryParse(mas[0], out byte S);
    bool third = byte.TryParse(mas[0], out byte V);
    bool result = (first && second && third) && ((H >= 0 && H <= 360) && (S >= 0 && S <= 100) && (V >= 0 && V <= 100));
    return result;
}
private bool CheckHsl(string[] mas)
{
    bool first = short.TryParse(mas[0], out short H);
    bool second = byte.TryParse(mas[0], out byte S);
    bool third = byte.TryParse(mas[0], out byte L);
    bool result = (first && second && third) && ((H >= 0 && H <= 360) && (S >= 0 && S <= 100) && (L >= 0 && L <= 100));
    return result;
}
private bool CheckXyz(string[] mas)
{
    bool first = byte.TryParse(mas[0], out byte X);
    bool second = byte.TryParse(mas[0], out byte Y);
    bool third = byte.TryParse(mas[0], out byte Z);
    bool result = (first && second && third) && ((X >= 0 && X <= 95) && (Y >= 0 && Y <= 100) && (Z >= 0 && Z <= 108));
    return result;
}
```
<br>
As a result, the application looks like this:

![](https://raw.githubusercontent.com/NikitaBeliayev/BSU-Computer-Graphic-Programming/development/Color%20Models/.media/img_3.png)

![](https://raw.githubusercontent.com/NikitaBeliayev/BSU-Computer-Graphic-Programming/development/Color%20Models/.media/img_0.png)

<br>
<br>
