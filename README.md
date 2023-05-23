### Hi there 👋
![](https://komarev.com/ghpvc/?username=LinusOlofsson-maker)

//
//  ClockWidget.swift
//  WidgetExtension
//
//  Created by Pawel Wiszenko on 15.10.2020.
//  Copyright © 2020 Pawel Wiszenko. All rights reserved.
//

import SwiftUI
import WidgetKit

private struct Provider: TimelineProvider {
    func placeholder(in context: Context) -> SimpleEntry {
        SimpleEntry(date: Date())
    }

    func getSnapshot(in context: Context, completion: @escaping (SimpleEntry) -> Void) {
        let entry = SimpleEntry(date: Date())
        completion(entry)
    }

    func getTimeline(in context: Context, completion: @escaping (Timeline<Entry>) -> Void) {
        var entries = [SimpleEntry]()
        let currentDate = Date()
        let midnight = Calendar.current.startOfDay(for: currentDate)
        let nextMidnight = Calendar.current.date(byAdding: .day, value: 1, to: midnight)!

        for offset in 0 ..< 60 * 24 {
            let entryDate = Calendar.current.date(byAdding: .minute, value: offset, to: midnight)!
            entries.append(SimpleEntry(date: entryDate))
        }

        let timeline = Timeline(entries: entries, policy: .after(nextMidnight))
        completion(timeline)
    }
}

private struct SimpleEntry: TimelineEntry {
    let date: Date
}

private struct ClockWidgetEntryView: View {
    var entry: Provider.Entry

    private static let dateFormatter: DateFormatter = {
        let formatter = DateFormatter()
        formatter.locale = Locale(identifier: "en_US_POSIX")
        formatter.dateFormat = "HH:mm"
        return formatter
    }()

    var body: some View {
        VStack {
            Text("\(entry.date, formatter: Self.dateFormatter)")
            Text(entry.date, style: .time)
        }
    }
}

struct ClockWidget: Widget {
    private let kind: String = WidgetKind.clock

    var body: some WidgetConfiguration {
        StaticConfiguration(kind: kind, provider: Provider()) { entry in
            ClockWidgetEntryView(entry: entry)
        }
        .configurationDisplayName("Clock Widget")
        .description("A Widget that displays the current time (in various formats) and refreshes every minute.")
        .supportedFamilies([.systemSmall])
    }
}
<!--
**LinusOlofsson-maker/LinusOlofsson-maker** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

![Current UTC time](https://jojoee.jojoee.com/api/utcnowgif?utcnow)
Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->

