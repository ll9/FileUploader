## Upload Files in ASP.Net Core

```c#
// Inject IWebHostEnvironment
public WeatherForecastController(ILogger<WeatherForecastController> logger, IWebHostEnvironment environment)

[HttpPost("UploadFiles")]
public async Task<IActionResult> Post(List<IFormFile> files)
{
    // do other validations on your model as needed
    foreach (var formFile in files)
    {
        var uniqueFileName = GetUniqueFileName(formFile.FileName);
        var uploads = Path.Combine(environment.ContentRootPath, "uploads");
        var filePath = Path.Combine(uploads, uniqueFileName);

        var newfile = new FileStream(filePath, FileMode.CreateNew);
        formFile.CopyTo(newfile); 
        newfile.Close();

        //to do : Save uniqueFileName  to your db table   
    }
    return Ok(new { count = files.Count, size = files.Sum(f => f.Length) });
}

private string GetUniqueFileName(string fileName)
{
    fileName = Path.GetFileName(fileName);
    return Path.GetFileNameWithoutExtension(fileName)
                + "_"
                + Guid.NewGuid().ToString().Substring(0, 4)
                + Path.GetExtension(fileName);
}
```

![](assets/2019-10-06-20-14-05.png)