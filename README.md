# Win32 DPI-aware window example

Trivial example on how to write Win32 DPI-aware GUI application that scales properly on everything starting Windows XP and ending with latest Windows 10 (tested on insider build 10.0.18980.1).

## In a nutshell

* Primary monitor is usually at System DPI (unless changed without restart)
* Other monitors can be at higher or lower DPI than System
* All metrics normally (pre-v2) reported to the application are in System DPI
* Some APIs (GetThemeFont) scale the reported values when in PerMonitorV2 awareness mode
* It's error-prone to call GetSystemMetrics wherever needed and scale manually, precompute these
* Mostly everything derives metrics from font size: recreate fonts on DPI change, remember height, rescale accordingly
* Window and Taskbar icons sizes change too

## Manifest

For the application to support DPI scaling to the full extent of what the underlying Operating System supports, the process DPI awareness must be set.
That's accomplished either through manifest, or calling API(s). The API way is complicated. We will use my **[rsrcgen.exe](https://github.com/tringi/rsrcgen)**
tool to generate manifest that will request everything known so far, and then deal with what we get at runtime.

### API alternative:
* [SetProcessDPIAware](https://docs.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-setprocessdpiaware) - Windows Vista / Server 2008
* [SetProcessDpiAwareness](https://docs.microsoft.com/cs-cz/windows/win32/api/shellscalingapi/nf-shellscalingapi-setprocessdpiawareness) - Windows 8.1 / Server 2012 R2
* [SetProcessDpiAwarenessContext](https://docs.microsoft.com/cs-cz/windows/win32/api/winuser/nf-winuser-setprocessdpiawarenesscontext) - Windows 10 1607 / LTSB 2016 / Server 2016
   - with [DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2](https://docs.microsoft.com/cs-cz/windows/win32/hidpi/dpi-awareness-context) - Windows 10 1703
   - with [DPI_AWARENESS_CONTEXT_UNAWARE_GDISCALED](https://docs.microsoft.com/cs-cz/windows/win32/hidpi/dpi-awareness-context) - Windows 10 1809 / LTSC 2019 / Server 2019

## Additional reading

* [High DPI Desktop Application Development on Windows](https://docs.microsoft.com/cs-cz/windows/win32/hidpi/high-dpi-desktop-application-development-on-windows?redirectedfrom=MSDN)
* [Mixed-Mode DPI Scaling and DPI-aware APIs](https://docs.microsoft.com/cs-cz/windows/win32/hidpi/high-dpi-improvements-for-desktop-applications)
* [Setting the default DPI awareness for a process](https://docs.microsoft.com/cs-cz/previous-versions/windows/desktop/legacy/mt846517)
* [Improving the high-DPI experience in GDI based Desktop Apps](https://blogs.windows.com/windowsdeveloper/2017/05/19/improving-high-dpi-experience-gdi-based-desktop-apps/#VllxTW8vOXIB4HqW.97)
* [High-DPI Scaling Improvements for Desktop Applications in the Windows 10 Creators Update (1703)](https://blogs.windows.com/windowsdeveloper/2017/04/04/high-dpi-scaling-improvements-desktop-applications-windows-10-creators-update/#Due3kFlj32WEmiwf.97)
* [High DPI Scaling Improvements for Desktop Applications and �Mixed Mode� DPI Scaling in the Windows 10 Anniversary Update (1607)](https://blogs.windows.com/windowsdeveloper/2016/10/24/high-dpi-scaling-improvements-for-desktop-applications-and-mixed-mode-dpi-scaling-in-the-windows-10-anniversary-update/#2rElZ4ZhV2dvcUOp.97)

## TODO

* Extend icon to include all realistic sizes
