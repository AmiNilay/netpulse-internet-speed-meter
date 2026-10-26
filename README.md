# NetPulse / SpeedMeter — Telemetry Suite & Technical Specification

> **Version**: 2.5 (Neo-Minimalist Redesign & Pro Diagnostic Suite)  
> **Target Framework**: Native Android (Kotlin / Material 3 / Custom Canvas)  
> **Minimum SDK**: Android 7.0 (API Level 24) | **Target SDK**: Android 14 (API Level 34)  
> **Design Language**: Neo-Minimalist (24dp Card Radius, 1dp Hairline Borders, Critically Damped Springs)  
> **Telemetry Model**: 100% Client-Side On-Device Processing | Zero Cloud Overhead  

---

## 1. Design Philosophy & Visual Identity

NetPulse follows a **Modern Neo-Minimalist** aesthetic characterized by:
* **Generous White Space & High Contrast**: Dark charcoal (`#111619`) text on pure white canvas (`#FFFFFF`) in light mode; OLED dark (`#0E1215`) in dark mode.
* **Warm Accent Highlights**: Vibrant coral/salmon (`#FF6B58`) for active states and critical alerts; warm ivory/cream (`#F6F4ED`) for secondary pill buttons and badges; emerald green (`#34C759`) for active connection health and fast latency.
* **Subtle Hairline Borders Instead of Heavy Shadows**: Cards use flat surfaces with `1dp` subtle borders (`#F0F1F3` light / `#202A31` dark) and zero elevation, delivering a crisp, modern feel.
* **Rounded Geometry**: Deep `24dp` rounded corners on cards and full `999dp` capsules for buttons, tabs, and status badges.
* **Fluid Physics & Instant Feedback**: Every touch interaction responds immediately on pointer-down with tactile spring scaling (`scale(0.96)`) using critically damped curves (damping ratio `1.0`).

---

## 2. Color Palette & Visual Tokens

| Token Name | Light Mode HEX | Dark Mode HEX | Semantic Role |
| :--- | :--- | :--- | :--- |
| `primary_dark_charcoal` | `#111619` | `#FFFFFF` | Primary headers, display numbers, dial progress arc |
| `accent_coral` | `#FF6B58` | `#FF7565` | Active alerts, high-congestion warnings, throttle tags |
| `accent_coral_light` | `#FFF0EE` | `#2A1614` | Subtle tinted background for alert chips |
| `accent_emerald` | `#34C759` | `#30D158` | Active connection badges, low latency indicators, healthy status |
| `canvas_background` | `#FFFFFF` | `#0E1215` | Default screen background canvas |
| `canvas_speedtest_cream` | `#F5F4EE` | `#12171B` | Warm parchment canvas for gauge and diagnostic views |
| `card_surface` | `#FFFFFF` | `#161D22` | Card body surface |
| `card_border` | `#F0F1F3` | `#202A31` | `1dp` hairline borders for cards and row dividers |
| `pill_cream_bg` | `#F6F4ED` | `#1E252B` | Secondary pill buttons, metric pods, and history chips |
| `gauge_arc_active` | `#111619` | `#FFFFFF` | Gauge dial filled progress arc |
| `gauge_arc_track` | `#D4D8DC` | `#2A353E` | Gauge dial background track |
| `text_primary` | `#111619` | `#FFFFFF` | High-emphasis headers and primary metric values |
| `text_secondary` | `#8A919B` | `#8E98A0` | Subtitles, measurement units, day labels, chevron icons |

---

## 3. Typography Scale

All typography uses the platform geometric sans-serif font family (`Inter` / `SF Pro` / `sans-serif`).

| Style Level | Size (sp) | Weight | Letter Spacing | Line Height | Usage Example |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Display Large** | `34sp` | Bold (700) | `-0.5sp` | `40sp` | `120 GB`, `45.2 MB/s` primary counters |
| **Screen Title** | `22sp` | Bold (700) | `-0.2sp` | `28sp` | `Data Usage`, `Settings`, `Diagnostics` |
| **Section Header** | `17sp` | SemiBold (600) | `0sp` | `22sp` | `Pro Diagnostic Suite`, `Global Settings` |
| **Metric Bold** | `18sp` | Bold (700) | `0sp` | `24sp` | `45 GB`, `18 GB` metric cards |
| **Body Primary** | `15sp` | Medium (500) | `0sp` | `20sp` | Row titles, network names, hostnames |
| **Body Secondary** | `13sp` | Regular (400) | `0.1sp` | `18sp` | `Remaining Quota`, `Subnet 192.168.1.0/24` |
| **Caption/Badge** | `11sp` | SemiBold (600) | `0.2sp` | `14sp` | `GATEWAY`, `ACTIVE DATA`, `99.9% UPTIME` |

---

## 4. Spacing & Layout Tokens

* **Base Grid Unit**: `4dp`
* **Screen Edge Margins**: `20dp` (breathable layout padding)
* **Card Internal Padding**: `20dp` (compact items: `16dp`)
* **Card Corner Radius**: `24dp` (Neo-Minimalist Card Style)
* **Pill/Button Radius**: `999dp` (capsule geometry)
* **Card Border Stroke**: `1dp` solid `@color/card_border`
* **Card Elevation**: `0dp` (replaces heavy skeuomorphic drop shadows with crisp hairline strokes)

---

## 5. Architectural & System Engineering

* **Native Modern Kotlin Core**: Built with idiomatic Kotlin, Kotlin Coroutines, StateFlow, and Jetpack ViewBinding.
* **Direct Kernel Querying**: Direct interface reads via `/proc/net/dev`, `/proc/net/arp`, and Android `NetworkStatsManager` ensure near-zero CPU wakeups and under 1% daily battery draw.
* **Silent Status Bar Service**: Low-priority foreground `dataSync` service generating custom runtime canvas bitmaps directly into the status bar, eliminating disruptive heads-up popups.
* **100% Free & Local Processing**: Zero external cloud dependencies, zero telemetry tracking, zero monthly costs.

---

## 6. Pro Diagnostic Suite & Feature Modules

### 6.1 Silent Status Bar Telemetry
* **Mechanism**: Generates dynamic canvas text bitmaps in real time representing current transfer rates.
* **Customization**: User-selectable between decimal megabits per second (`Mbps`) and binary megabytes per second (`MB/s`).

### 6.2 240-Degree Calibrated Gauge Accelerator
* **Custom Canvas View**: `CustomGaugeMeter` sweeping 260 degrees (140° to 400°) with milestone ticks and decelerated indicator needle.
* **Mode Toggle**: Immediate spring-animated switching between Download and Upload monitoring modes.

### 6.3 Per-App "Data Hog" & Background Leak Detector
* **Activity**: `AppUsageActivity` (`res/layout/activity_app_usage.xml`)
* **Manager**: `AppUsageManager` querying `NetworkStatsManager` with `PACKAGE_USAGE_STATS`.
* **Telemetry**: Isolates foreground app usage from silent background leaks across Today, 7-day, and 30-day time horizons.

### 6.4 Floating Speed Bubble
* **Service**: `FloatingBubbleService` (`res/layout/bubble_speed_overlay.xml`)
* **Mechanism**: Operates via `WindowManager` (`TYPE_APPLICATION_OVERLAY`) with spring drag physics and auto-snapping to screen edges.

### 6.5 Smart Quota Predictor & Daily Allowance
* **Model**: `QuotaForecast` via `DataPreferences.getSmartQuotaForecast()`.
* **Intelligence**: Computes diurnal burn pace against user billing cycle renewal day, predicting exact quota exhaustion days.

### 6.6 Wi-Fi Security & Channel Congestion Analyzer
* **Integrated**: `DiagnosticsActivity`
* **Checks**: Protocol validation (WPA3-SAE, WPA2-PSK, Open), central frequency conversion (2.4 GHz, 5 GHz, 6 GHz), and co-channel interference assessment.

### 6.7 DNS Speed Benchmark & Private DNS Optimizer
* **Activity**: `DnsBenchmarkActivity` (`res/layout/activity_dns_benchmark.xml`)
* **Engines**: Measures socket response times against Cloudflare (1.1.1.1), Google (8.8.8.8), Quad9 (9.9.9.9), and ISP gateway; provides 1-tap shortcut to Android Private DNS.

### 6.8 Pro Diagnostic Tool 1: ISP Outage & Micro-Drop Logger
* **Activity**: `OutageLogActivity` (`res/layout/activity_outage_log.xml`)
* **Engine**: `OutageDetector` running silent 30-second passive socket heartbeats.
* **Intelligence**: Detects micro-disconnects where Wi-Fi shows connected but packets drop. Computes 24h stability scores and exports certified customer support audit reports.

### 6.9 Pro Diagnostic Tool 2: Gaming Latency & Bufferbloat Radar
* **Activity**: `GamingRadarActivity` (`res/layout/activity_gaming_radar.xml`)
* **Engine**: `BufferbloatTester` benchmarking latency variance across Discord RTC, AWS, and Cloudflare.
* **Grading**: Measures queue delay delta under saturated download/upload load, grading router queue health from A+ down to F.

### 6.10 Pro Diagnostic Tool 3: Wi-Fi Dead-Zone Room Mapper
* **Activity**: `WifiHeatmapActivity` (`res/layout/activity_wifi_heatmap.xml`)
* **Engine**: `WifiSurveyManager` sampling localized RSSI (dBm), link speed, and frequency per room.
* **Diagnostics**: Evaluates physical wall attenuation caused by concrete/drywall and generates mesh repeater placement advice.

### 6.11 Pro Diagnostic Tool 4: Peak-Hour ISP Throttling Detector
* **Activity**: `IspThrottleActivity` (`res/layout/activity_isp_throttle.xml`)
* **Engine**: `ThrottleDetector` evaluating diurnal throughput (Morning, Afternoon, Evening Peak, Night Off-Peak).
* **Intelligence**: Quantifies bandwidth drops and latency surges during evening peak hours (18:00 - 23:00) to expose provider traffic shaping.

### 6.12 Pro Diagnostic Tool 5: LAN Device Scanner ("Who is on my Wi-Fi?")
* **Activity**: `LanScannerActivity` (`res/layout/activity_lan_scanner.xml`)
* **Engine**: `LanScanner` sweeping local `/24` subnet with concurrent socket probes and `/proc/net/arp` cache resolution.
* **Telemetry**: Categorizes hosts (Router, Phone, Laptop, TV, IoT) with MAC address identification and round-trip ping latency.

### 6.13 Pro Diagnostic Tool 6: Dual-SIM & Roaming Data Guard
* **Activity**: `DualSimActivity` (`res/layout/activity_dual_sim.xml`)
* **Manager**: `SimDataManager` interfacing with Android `SubscriptionManager` and `TelephonyManager`.
* **Shield**: Active background protection alerting when cellular connection switches to international roaming to prevent accidental fees.

### 6.14 Home Screen Desktop Widgets
* **Providers**: `SpeedWidgetCompactProvider` (2x2) and `SpeedWidgetWideProvider` (4x2).
* **Sync**: Live broadcast sync with `SpeedMeterService` whenever network packets transfer.

### 6.15 Shareable "Speed Flex" Card
* **Integrated**: `DiagnosticsActivity.shareSpeedFlexCard()`
* **Mechanism**: Renders high-resolution image of the speedometer gauge and network metrics, exported via Android `FileProvider` to social platforms.

---

## 7. Build Commands

```bash
# Compile Debug APK
./gradlew assembleDebug

# Compile Optimized Production Release APK
./gradlew assembleRelease

# Clean Build Cache
./gradlew clean
```

---

## 8. License

Developed and maintained by Nilay Naha.
Licensed under the Apache License 2.0.
