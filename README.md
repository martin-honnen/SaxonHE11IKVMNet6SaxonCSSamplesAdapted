# SaxonHE11IKVMNet6SaxonCSSamplesAdapted
SaxonCS examples adapted to be used with Saxon HE 11 cross-compiled to .NET 6 using IKVM


The basic usage is to to install the NuGet package IKVM.Maven.Sdk to be able to pull in the Saxon HE 11 (e.g. 11.4) and the XmlResolver code it uses directly from Maven:
```
  <ItemGroup>
    <PackageReference Include="IKVM.Maven.Sdk" Version="1.0.1" />
    <MavenReference Include="net.sf.saxon:Saxon-HE" version="11.4" />
    <MavenReference Include="org.xmlresolver:xmlresolver" Version="4.5.1" />
    <MavenReference Include="org.xmlresolver:xmlresolver" Category="data" Version="4.5.1" />
  </ItemGroup>
```

My extension project https://github.com/martin-honnen/SaxonHE11s9apiExtensions is also on NuGet so you can add it in your project e.g.

```
  <ItemGroup>
    <PackageReference Include="IKVM.Maven.Sdk" Version="1.0.1" />
    <PackageReference Include="SaxonHE11s9apiExtensions" Version="11.4.0-alpha4" />
    <MavenReference Include="net.sf.saxon:Saxon-HE" version="11.4" />
    <MavenReference Include="org.xmlresolver:xmlresolver" Version="4.5.1" />
    <MavenReference Include="org.xmlresolver:xmlresolver" Category="data" Version="4.5.1" />
  </ItemGroup>
```

Then you are ready to write .NET 6 code against the Saxon 11 s9api API, helped by the extension library to not have to use most Java classes for input/output/URIs/URLs but to be able to use relevant .NET classes:

```
using net.sf.saxon.s9api;
using net.liberty_development.SaxonHE11s9apiExtensions;
using System.Reflection;

// force loading of updated xmlresolver
ikvm.runtime.Startup.addBootClassPathAssembly(Assembly.Load("org.xmlresolver.xmlresolver"));
ikvm.runtime.Startup.addBootClassPathAssembly(Assembly.Load("org.xmlresolver.xmlresolver_data"));

var processor = new Processor(false);

Console.WriteLine($"{processor.getSaxonEdition()} {processor.getSaxonProductVersion()}");

var xslt30Transformer = processor.newXsltCompiler().Compile(new Uri("https://github.com/martin-honnen/martin-honnen.github.io/raw/master/xslt/processorTestHTML5Xslt3InitialTempl.xsl")).load30();

xslt30Transformer.callTemplate(null, processor.NewSerializer(Console.Out));
```
