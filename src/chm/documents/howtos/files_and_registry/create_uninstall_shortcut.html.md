---
title: How To: Create an Uninstall Shortcut
layout: documentation
---
# How To: Create an Uninstall Shortcut
When installing an application it is a common requirement to place a shortcut on the user&apos;s Start Menu to provide a method of uninstalling the application. This how to demonstrates the steps required to create an uninstall shortcut on the start menu that passes all ICE validation checks.

This how to assumes you are starting with the sample described the [How To: Create a Shortcut on the Start Menu](create_start_menu_shortcut.html) topic.

## Step 1: Add the Uninstall Shortcut
The [&lt;Shortcut&gt;](~/xsd/wix/shortcut.html) element is used to add the uninstall shortcut to the start menu, and the shortcut points to msiexec.exe (the Windows Installer executable used to actually invoke the uninstall process). Anywhere within the existing ApplicationShortcut component add the following:

<pre>
<font size="2"><font color="#0000FF">&lt;</font><font color="#A31515">Shortcut</font><font color="#FF0000"> Id</font><font color="#0000FF">=</font>"<font color="#0000FF">UninstallProduct</font>" <font color="#FF0000">            
          Name</font><font color="#0000FF">=</font>"<font color="#0000FF">Uninstall My Application</font>"<font color="#FF0000">
          Target</font><font color="#0000FF">=</font>"<font color="#0000FF">[SystemFolder]msiexec.exe</font>"<font color="#FF0000">
          Arguments</font><font color="#0000FF">=</font>"<font color="#0000FF">/x [ProductCode]</font>"<font color="#FF0000">
          Description</font><font color="#0000FF">=</font>"<font color="#0000FF">Uninstalls My Application</font>"<font color="#0000FF"> /&gt;</font></font>
</pre>

The Target attribute points to the location of msiexec.exe. The Windows Installer <a target="_blank" href="http://msdn.microsoft.com/en-us/library/aa372055.aspx">SystemFolder</a> property will resolve to the *System32* directory where msiexec.exe resides. The Arguments attribute is used to let msiexec.exe know which product to uninstall by passing in the ProductCode for the install package.

To avoid ICE validation errors at build it is important to couple the Shortcut element with a registry entry and a RemoteFolder element. Both of these are described in more detail in the [How To: Create a Shortcut on the Start Menu](create_start_menu_shortcut.html) topic, and are shown in the complete sample below.

## The Complete Sample
The following is a complete sample that uses the above concepts. This example can be inserted into a WiX project and compiled, or compiled and linked from the command line, to generate an installer.

<pre>
<font size="2" color="#0000FF">&lt;?</font><font size="2" color="#A31515">xml</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">version</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">1.0</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">encoding</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">UTF-8</font><font size="2">"</font><font size="2" color="#0000FF">?&gt;
&lt;<font size="2" color="#A31515">Wix</font> <font size="2" color="#FF0000">xmlns</font>=<font size="2">"</font>http://schemas.microsoft.com/wix/2006/wi<font size="2">"</font>&gt;
    &lt;<font size="2" color="#A31515">Product</font> <font size="2" color="#FF0000">Id</font>=<font size="2">"*"</font> <font size="2" color="#FF0000">UpgradeCode</font>=<font size="2">"</font></font><a href="~/howtos/general/generate_guids.html"><font size="2">PUT-GUID-HERE</font></a><font size="2" color="#0000FF"><font size="2">"</font> <font size="2" color="#FF0000">Version</font>=<font size="2">"1.0.0.0" </font><font size="2" color="#FF0000">Language</font>=<font size="2">"</font>1033<font size="2">" </font><font size="2" color="#FF0000">Name</font>=<font size="2">"My Application Name" </font><font size="2" color="#FF0000">Manufacturer</font>=<font size="2">"My Manufacturer Name"</font>&gt;
        &lt;<font size="2" color="#A31515">Package</font> <font size="2" color="#FF0000">InstallerVersion</font>=<font size="2">"</font>300<font size="2">"</font> <font size="2" color="#FF0000">Compressed</font>=<font size="2">"</font>yes<font size="2">"</font>/&gt;
        &lt;<font size="2" color="#A31515">Media</font> <font size="2" color="#FF0000">Id</font>=<font size="2">"</font>1<font size="2">"</font> <font size="2" color="#FF0000">Cabinet</font>=<font size="2">"myapplication</font>.cab<font size="2">"</font> <font size="2" color="#FF0000">EmbedCab</font>=<font size="2">"</font>yes<font size="2">"</font> /&gt;

        &lt;</font><font size="2" color="#A31515">Directory</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Id</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">TARGETDIR</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Name</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">SourceDir</font><font size="2">"</font><font size="2" color="#0000FF">&gt;
            &lt;</font><font size="2" color="#A31515">Directory</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Id</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">ProgramFilesFolder</font><font size="2">"</font><font size="2" color="#0000FF">&gt;
                &lt;</font><font size="2" color="#A31515">Directory</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Id</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">APPLICATIONROOTDIRECTORY</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Name</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">My Application Name</font><font size="2">"</font><font size="2" color="#0000FF">/&gt;
            &lt;/</font><font size="2" color="#A31515">Directory</font><font size="2" color="#0000FF">&gt;
            &lt;</font><font size="2" color="#A31515">Directory</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Id</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">ProgramMenuFolder</font><font size="2">"</font><font size="2" color="#0000FF">&gt;
                &lt;</font><font size="2" color="#A31515">Directory</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Id</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">ApplicationProgramsFolder</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Name</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">My Application Name</font><font size="2">"</font><font size="2" color="#0000FF">/&gt;
            &lt;/</font><font size="2" color="#A31515">Directory</font><font size="2" color="#0000FF">&gt;
        &lt;/</font><font size="2" color="#A31515">Directory</font><font size="2" color="#0000FF">&gt;

        &lt;</font><font size="2" color="#A31515">DirectoryRef</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Id</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">APPLICATIONROOTDIRECTORY</font><font size="2">"</font><font size="2" color="#0000FF">&gt;
            &lt;</font><font size="2" color="#A31515">Component</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Id</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">myapplication.exe</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Guid</font><font size="2" color="#0000FF">=</font><font size="2">"<a href="~/howtos/general/generate_guids.html">PUT-GUID-HERE</a>"</font><font size="2" color="#0000FF">&gt;
                &lt;</font><font size="2" color="#A31515">File</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Id</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">myapplication.exe</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Source</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">MySourceFiles\MyApplication.exe</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">KeyPath</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">yes</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Checksum</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">yes</font><font size="2">"</font><font size="2" color="#0000FF">/&gt;
            &lt;/<font size="2" color="#A31515">Component</font>&gt;
            &lt;<font size="2" color="#A31515">Component</font> <font size="2" color="#FF0000">Id</font>=<font size="2">"d</font>ocumentation.<font size="2">html"</font> <font size="2" color="#FF0000">Guid</font>=<font size="2">"</font></font><a href="~/howtos/general/generate_guids.html"><font size="2">PUT-GUID-HERE</font></a><font size="2" color="#0000FF"><font size="2">"</font>&gt;
                &lt;</font><font size="2" color="#A31515">File</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Id</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">documentation.html</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Source</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">MySourceFiles\documentation.html</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">KeyPath</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">yes</font><font size="2">"</font><font size="2" color="#0000FF">/&gt;
            &lt;/<font size="2" color="#A31515">Component</font>&gt;
        &lt;/</font><font size="2" color="#A31515">DirectoryRef</font><font size="2" color="#0000FF">&gt;

        &lt;</font><font size="2" color="#A31515">DirectoryRef</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Id</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">ApplicationProgramsFolder</font><font size="2">"</font><font size="2" color="#0000FF">&gt;
            &lt;</font><font size="2" color="#A31515">Component</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Id</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">ApplicationShortcut</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Guid</font><font size="2" color="#0000FF">=</font><font size="2">"<a href="~/howtos/general/generate_guids.html">PUT-GUID-HERE</a>"</font><font size="2" color="#0000FF">&gt;
                &lt;</font><font size="2" color="#A31515">Shortcut</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Id</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">ApplicationStartMenuShortcut</font><font size="2">"</font><font size="2" color="#0000FF"> <br />                     </font><font size="2" color="#FF0000">Name</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">My Application Name</font><font size="2">"</font><font size="2" color="#0000FF"> <br />                   </font><font size="2" color="#FF0000">Description</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">My Application Description</font><font size="2">"<br />                    </font><font size="2" color="#FF0000">Target</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">[#myapplication.exe]</font><font size="2">"
                          </font><font size="2" color="#FF0000">WorkingDirectory</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">APPLICATIONROOTDIRECTORY</font><font size="2">"</font><font size="2" color="#0000FF">/&gt;
                &lt;!--</font><font size="2" color="#008000"> Step 1: Add the uninstall shortcut to your installer package </font><font size="2" color="#0000FF">--&gt;
                </font><font size="2"><font color="#0000FF">&lt;</font><font color="#A31515">Shortcut</font><font color="#FF0000"> Id</font><font color="#0000FF">=</font>"<font color="#0000FF">UninstallProduct</font>" <font color="#FF0000">            
                          Name</font><font color="#0000FF">=</font>"<font color="#0000FF">Uninstall My Application</font>"<font color="#FF0000">
                          Description</font><font color="#0000FF">=</font>"<font color="#0000FF">Uninstalls My Application</font>"<font color="#FF0000">
                          Target</font><font color="#0000FF">=</font>"<font color="#0000FF">[System64Folder]msiexec.exe</font>"<font color="#FF0000">
                          Arguments</font><font color="#0000FF">=</font>"<font color="#0000FF">/x [ProductCode]</font>"<font color="#0000FF">/&gt;</font></font><font size="2" color="#0000FF">
                &lt;</font><font size="2" color="#A31515">RemoveFolder</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Id</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">ApplicationProgramsFolder</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">On</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">uninstall</font><font size="2">"</font><font size="2" color="#0000FF">/&gt;
                &lt;</font><font size="2" color="#A31515">RegistryValue</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Root</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">HKCU</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Key</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">Software\Microsoft\MyApplicationName</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Name</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">installed</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Type</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">integer</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Value</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">1</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">KeyPath</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">yes</font><font size="2">"</font><font size="2" color="#0000FF">/&gt;<br />           &lt;/</font><font size="2" color="#A31515">Component</font><font size="2" color="#0000FF">&gt;
        &lt;/</font><font size="2" color="#A31515">DirectoryRef</font><font size="2" color="#0000FF">&gt;

        &lt;</font><font size="2" color="#A31515">Feature</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Id</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">MainApplication</font><font size="2">"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Title</font><font size="2" color="#0000FF">=</font><font size="2">"Main Application"</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Level</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">1</font><font size="2">"</font><font size="2" color="#0000FF">&gt;
            &lt;</font><font size="2" color="#A31515">ComponentRef</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Id</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">myapplication.exe</font><font size="2">"</font><font size="2" color="#0000FF"> /&gt;
            &lt;</font><font size="2" color="#A31515">ComponentRef</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Id</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">documentation.html</font><font size="2">"</font><font size="2" color="#0000FF"> /&gt;
            &lt;</font><font size="2" color="#A31515">ComponentRef</font><font size="2" color="#0000FF"> </font><font size="2" color="#FF0000">Id</font><font size="2" color="#0000FF">=</font><font size="2">"</font><font size="2" color="#0000FF">ApplicationShortcut</font><font size="2">"</font><font size="2" color="#0000FF"> /&gt;   
        &lt;/</font><font size="2" color="#A31515">Feature</font><font size="2" color="#0000FF">&gt;
    &lt;/</font><font size="2" color="#A31515">Product</font><font size="2" color="#0000FF">&gt;
&lt;/</font><font size="2" color="#A31515">Wix</font><font size="2" color="#0000FF">&gt;</font>
</pre>