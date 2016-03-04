# test

<pre>
<code class="codepad language-csharp" data-codepad="theme:xcode" style="width:100%;height:400px;" >int state = 3;
try
{
    throw new InvalidOperationException();
}
catch (InvalidOperationException) when (state == 2)
{
    Console.WriteLine("Exception thrown! State = 2");
}
catch (InvalidOperationException) when (state == 3)
{
    Console.WriteLine("Exception thrown! State = 3");
}
catch
{
    Console.WriteLine("Exception thrown!");
}

//--------

</code>
</pre>



<script src="//csharppad.com/embed.js"></script>
<script type="text/javascript"> Codepad.all(); </script>
