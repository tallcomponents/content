# Progressive rendering

As of version 3.0 it is possible to render PDF documents aynchronously and retrieve intermediate bitmaps. This allows you to render a bitmap progressively. The central class is DrawJob.



## Basic code sample

The following code sample starts an asynchronous draw job and renders intermediate results.


```
static void Main(string[] args)
{
   using (FileStream file = new FileStream(
      @"docment.pdf", FileMode.Open, FileAccess.Read))
   {
      Document document = new Document(file);
      Page page = document.Pages[10];

      using (Bitmap bitmap = new Bitmap((int)page.Width, (int)page.Height))
      {
         DrawJob drawJob = new DrawJob(page);
         drawJob.Updated += new EventHandler(drawJob_Updated);
         drawJob.Start(bitmap, new RenderSettings(), 
            PageBoundary.Default, new Matrix());

         // this call blocks until the job has finished
         drawJob.Wait();

         bitmap.Save(@"bitmap_ready.png", ImageFormat.Png);
      }
   }  
}

static void drawJob_Updated(object sender, EventArgs e)
{
   DrawJob drawJob = sender as DrawJob;
   using (Bitmap copy = drawJob.GetBitmap())
   {
      string path = string.Format(@"bitmap_{0}.png",
         DateTime.Now.Ticks);
      copy.Save(path, ImageFormat.Png);
   }
}
```

Code sample: Render a page asynchronously and save intermediate bitmaps



## Updated event

As shown in the previous code sample, it is possible to subscribe to the Updated event.


This event is raised regularly while the job is running and immediately after drawing has finished. This event is provided as a convenience so that the generated bitmap can be inspected regularly, without needing to implement this functionality oneself. As a rule of thumb, this event fires at a higher frequency for simple drawing jobs, so that results can be shown quickly, and at a lower frequency for complex jobs, so that the drawing job itself will not be slowed down too much by frequent updates.


The sender argument of the event handler can be cast to a DrawJob object. Inside this handler the JobStatus property indicates the status of the drawing job.



## Cancel drawing

You may cancel a drawing job while in progress by calling the DrawJob.Stop method. You typically call this method from an event handler of a UI control such as a Cancel button. The job may not stop immediately, but at a moment that permits it.



## Code samples

The following code samples are included in the distribution and are relevant to rendering PDF documents asynchronously:
&nbsp;<ul><li>
ViewPDF_Progressive</li></ul>&nbsp;
