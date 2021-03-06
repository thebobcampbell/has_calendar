has\_calendar
============

has\_calendar is a view helper that creates a calendar using a table. You can
easily add events with any content.

This plugin no longer requires the `cal` command and is therefore useable on
all systems ruby runs on. Furthermore the first-day-of-the-week problem is
gone as the plugin relies on ruby and is inheriting its settings.


Instalation
-----------

1) Install the plugin with `script/plugin install git://github.com/KieranP/has_calendar.git`

Usage
-----

If you want a very simple calendar, then all you need to do is:

    <%= calendar %>

If you want to display a specific year and month, you can provide them to the options.

    <%= calendar :year => 2009, :month => 8 %>

or, if you want to register some events:

    <% calendar :year => 2009, :month => 8 do |date| %>
      <% Schedule.all(:conditions => "date LIKE '%#{date}%'").each do |event| %>
        <%= link_to event.title, event_path(event) %>
      <% end %>
    <% end %>

As you can see, this will hit your database up to 31 times (one hit for each
day) if you don't optimize it. Fortunately, you can use the options `:events`:

    <% calendar :events => Schedule.all do |date, events| %>
      <% events.each do |event| %>
        <%= link_to event.title, event_path(event) %>
      <% end %>
    <% end %>

By default, each record will use the `created_at` attribute as date grouping.
You can specify a different attribute with the option `:field`:

    <% calendar :events => Schedule.all, :field => :scheduled_at do |date, events| %>
      <!-- loop through events -->
    <% end %>

By default, has\_calendar will use a three letter representation of the day name
to use on the header of the calendar. You can change it using the :header_format option:

    <%= calendar :header_format => :short_day_name %>
    <%= calendar :header_format => "%A" %>

You can also change the caption provided by has\_calendar (that defaults to the
:default format). Setting it to nil doesn't output a caption:

    <%= calendar :caption_format => :month_year %>
    <%= calendar :caption_format => nil %>

To set formats do something like this:

    Date::DATE_FORMATS[:short_day_name] = '%a' # => (Sun..Sat)

Or on your locale file, if your application is internationalized.

By default, calendars don't have ids set so multiple ones can be used on the page.
If you need an id, you can set the HTML id by doing the following:

    <%= calendar :id => 'cal' %>

The calendar has built in support for users to go to the previous or next month/year.
If you want to disable this support, you can set allow_user_date_select to false:

    <%= calendar :allow_user_date_select => false %>

The calendar has support for marking days that are important (deadlines for example).
You can mark days by doing the following:

    <%= calendar :marked => ["2009-08-07", "2009-09-20"] %>

An option, :start_week easily changes the week organization to Sun...Sat from the 
default Mon..Sun (per the beginning_of_week call).  Values are  0 (start on Monday) 
or -1 (start on Sunday), default is 0:  

	<%= calendar :start_week => -1 %>

For those who prefer the (prev|month|next) block, which is by default on the bottom, to be on
the top of the calendar, use the :place_footer option (default is "bottom"):

	<%= calendar :place_footer => "top" %>


Formatting the calendar
-----------------------

You can use this CSS to start:

    .calendar {
      border: 1px solid #555 !important;
      padding: 0px;
      border-spacing: 1px;
      margin: 10px 0;
    }

    .calendar td,
    .calendar th {
      color: #ccc;
      font-family: "Lucida Grande",arial,helvetica,sans-serif;
      font-size: 10px;
      padding: 6px;
      width: 50px;
      height: 50px;
    }

    .calendar th {
      border: 1px solid #ccc;
      border-bottom: 1px solid #555;
      border-right: 1px solid #555;
      background: #ccc;
      color: #666;
      text-align: left;
    }

    .calendar td {
      background-color: #F0F0F0;
      border: 1px solid #ddd;
      color: #000;
      vertical-align: text-top;
    }

    .calendar td.events {
      background: #fff;
    }

    .calendar td.today {
      background: #ffc;
      color: #666;
    }

    .calendar td.different_month {
      background: #E0E0E0;
      color: #666;
    }

    .calendar .sunday {
      color: maroon !important;
    }

    .calendar .marked {
      background-color: pink;
    }

    .calendar .footer th {
      height: auto;
      width: auto;
    }

    .calendar .footer .caption {
      text-align: center;
    }

    .calendar .footer .next_month {
      text-align: right;
    }

TO-DO
-----

- Write some specs as soon as possible!
- Convert to a class, with a helper that class Calendar.new.to_html
- With the new calendar class, remove the dependancy of as many Rails components as possible
- With the new calender class, provide a cleaner way to add marked days/events (cal.add_event, cal.add_marked)
- Allow notes to be attached to marked days


Contributors
------------

* Carlos Júnior (xjunior) <carlos@milk-it.net>
* Kieran Pilkington (KieranP) <kieran776@gmail.com>

Copyright (c) 2008 Nando Vieira, released under the MIT license
