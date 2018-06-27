# Diagnostics

It is possible to collect diagnostics about the render process by passing a Summary instance to the various render/conversion methods. This class builds a list of messages that can be inspected after the render process has finished.



## Basic code sample

The following code sample demonstrates how the Summary class provides diagnostics:


```
Summary summary = new Summary();
page.Draw(graphics, new RenderSettings(), summary);

foreach (Message message in summary.Messages)
{
   switch (message.Severity)
   {
      case MessageSeverity.Information:
         Console.WriteLine("Info: {0}", message.Text);
         break;
      case MessageSeverity.Warning:
         Console.WriteLine("Warning: {0}", message.Text);
         break;
      case MessageSeverity.Error:
         Console.WriteLine("Error: {0} | {1}", message.Text,
            message.Exception);
         break;
   }
}
```

Code sample: Dump render diagnostics to the output console


