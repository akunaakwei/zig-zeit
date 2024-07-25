# zeit

A time library written in zig.

## Usage

```zig
const std = @import("std");
const zeit = @import("zeit");

pub fn main() void {
    // Get an instant in time. The default gets "now" in UTC
    const now = zeit.instant(.{});

    // Load our local timezone. This needs an allocator
    const local = zeit.local(alloc);

    // Convert our instant to a new timezone
    const now_local = now.in(&local);

    // Generate date/time info for this instant
    const dt = now_local.time();

    // Print it out
    std.debug.print("{}", .{dt});

    // zeit.Time{
    //    .year = 2024,
    //    .month = zeit.Month.mar,
    //    .day = 16,
    //    .hour = 8,
    //    .minute = 38,
    //    .second = 29,
    //    .millisecond = 496,
    //    .microsecond = 706,
    //    .nanosecond = 64
    //    .offset = -18000,
    // }

    // Load an arbitrary location using IANA location syntax. The location name comes from an enum
    // which will automatically map IANA location names to Windows names, as needed
    const vienna = zeit.loadTimeZone(alloc, .@"Europe/Vienna");
    defer vienna.deinit();

    // Parse an Instant from an ISO8601 or RFC3339 string
    const iso = zeit.instant(.{
	.source = .{
	    .iso8601 = "2024-03-16T08:38:29.496-1200",
	},
    });

    const rfc3339 = zeit.instant(.{
	.source = .{
	    .rfc3339 = "2024-03-16T08:38:29.496706064-1200",
	},
    });
}
```
