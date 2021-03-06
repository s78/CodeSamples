### Regex
###### System.Text.RegularExpressions.Regex

Matches
``` csharp
foreach (Match match in Regex.Matches(result, @"(<pre>)(.*?)(</pre>)"))  // cannot use 'var' here..
{
    var innerText = WebUtility.HtmlDecode(match.Groups[2].Value);
    Console.WriteLine(innerText);
};
```

Replace
``` csharp
Regex.Replace(value, "<.*?>", "")
```

Replace (complex)
``` csharp
Regex.Replace(value, "<.*?>", m => m.Value.ToUpper())  // replace with all caps
```

"Singleline" Mode for Multiline Text Matching
``` csharp
var multilineText = String.Join("\r\n", "line1", "line2");
var matchFound = Regex.IsMatch(multilineText, "line.*?line", RegexOptions.Singleline);  // true  
```

> **Extremely confusing, but the dot expression does NOT work with 'RegexOptions.Multiline'**

Regex Escape
``` csharp
Regex.Escape("[12.34]")  // \[12\.34]
```

List All Supported Patterns
``` csharp
var patterns = new[]
{
    @"\w",
    @"\d",
    @"\s",
    @"\W",
    @"\D",
    @"\S",
    @".",
    @"[0-9]",
    @"[a-z]",
    @"[0-9A-Za-z]",
};

foreach (var p in patterns)
{
    Console.WriteLine(p);
    var cs = GetSupportedChars(p);
    new String(cs.Select(c => c <= 0x1f ? ' ' : c).ToArray()).Dump();
}

static char[] GetSupportedChars(string pattern)
{
    char maxChar = (char)127;  // limit to ascii
    var chars = Enumerable.Range(0, maxChar).Select(i => (char)i);
    return chars.Where(c => Regex.IsMatch(c.ToString(), pattern)).ToArray();
}
```

By Examples (C#)

|Description|Value|Pattern|Result|
|-----|-----|-----|-----|
|Extract index|"Labels[5]"|@"([a-zA-Z_][a-zA-Z0-9_]*\\[)(\d+)(\\])$"|"5"|
|Numeric value|"12","-12","12.5","-12.5",".5","-.5","-0.5"|@"(-\|)(\d+\.\d+\|\.\d+\|\d+)"|"12","-12","12.5","-12.5",".5","-.5","-0.5"|

