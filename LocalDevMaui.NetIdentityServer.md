# Communicating Between a Local .NET Backend and a Local MAUI App Running on Android Emulator

When developing a MAUI app that communicates with a .NET backend running locally, it's important to understand the different ways to connect the two. In this article, we'll explore the three main solutions to this problem.

![Development print screen where we see the changes in the .net backend, and the changes in the MAUI app](image.png)

## Solution 1: Use a Self-Signed Certificate

One solution is to use a self-signed certificate for both the .NET backend and the Android Emulator. However, this solution can be time-consuming and slow down the development process, so it's not recommended.

## Solution 2: Run Against the Backend in HTTP

Another solution is to run against the backend in HTTP. However, since Android 9.0 - API Level 28, you need to change some settings to allow clear HTTP traffic.

## Solution 3: Use HTTPS and Allow CORS Requests

The third and recommended solution is to use HTTPS and allow CORS requests in the .NET backend when running in debug mode or in dev settings. This can be achieved by using the right URL, as the Android Emulator is a VM and "localhost" redirects to its own network, not the host network (i.e. your laptop). Therefore, the solution is to use "10.0.2.2:port" instead.

To ensure that the Android Emulator can communicate with the .NET backend correctly, you should also ensure that the backend service allows CORS (Cross-Origin Resource Sharing) requests from the emulator's IP address:

```
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
        app.UseMigrationsEndPoint();
        app.UseCors(builder => builder.AllowAnyOrigin().AllowAnyMethod().AllowAnyHeader());
    }
    else
    {
        app.UseExceptionHandler("/Error");
        // The default HSTS value is 30 days. You may want to change this for production scenarios, see <https://aka.ms/aspnetcore-hsts>.
        app.UseHsts();
    }
		// Other configuration code ...
}

```

By following these guidelines, you can ensure that your MAUI app and .NET backend communicate correctly, making the development process smoother and more efficient.

## Summary

When developing a MAUI app that communicates with a .NET backend running locally, it's important to use the right approach. Using a self-signed certificate is not recommended, while running against the backend in HTTP can be problematic due to Android's security settings. The recommended approach is to use HTTPS and allow CORS requests in the .NET backend, ensuring that the Android Emulator can communicate with the backend correctly.