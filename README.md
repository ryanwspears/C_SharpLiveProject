# C# Live Project
Repo for my C# Live Project at The Tech Academy. It utilizes the C# language and the .NET framework. Visual Studio was the IDE that was used.

## Overview
My task for this project was to work alongside my fellow classmates to build a website for a Theater in Portland, OR. I was tasked with designing aand implementing a number of features. The first task was to style the Home Page per the clients request. This was done using simple HTML and CSS. Next, I was to create an entity model for calendar events that could be scheduled, and create the CRUD pages for it. Entity Framework made this a very simple task. After that, I had to link the calendar events to the production class. This was accomplished with a one-to-many relationship between the two classes where one production can have many calendar events. Then I was tasked with redesigning and styling the rental requests page. The previous design had the data displayed in a simple table. My job was to have the data display in an accordion. This one was a bit of a challenge, but was eventually overcome by some quick research. Lastly, I had to implement a way to only show current rental requests until the user clicked a button to show the expired requests. I had a tough time with this one, but like any other challenge it was quickly overcome. The solution was just a matter of using jQuery to handle the button function.

- [Home Page](#home-page)
- [Calendar Events](#calendar-events)
- [Rental Requests](#rental-requests)
- [Expired Requests](#expired-requests)

## Home Page
![Home Page GIF](/GIFs/homePage.gif)
### Code
```css
@font-face {
    font-family: "oswald-variablefont_wght";
    src: url("../webfonts/oswald-variablefont_wght.ttf") format("truetype");
}

.home-index--homeLogo {
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 50%;
  padding-top: 6em;
  padding-bottom: 8em;
}

.home-index--donateContainer {
  padding-top: 2em;
  padding-bottom: 2em;
}

.home-index--donationBtn {
  background-color: var(--main-color);
  border-radius: 8px;
  width: 10em;
  height: 3em;
  border-color: var(--dark-color);
  color: var(--light-color);
}

.home-index--donationBtn:hover {
  background-color: var(--main-color--light);
  text-decoration: none;
  color: var(--light-color);
}

.home-index--sponserLogos {
  width: 4em;
}

.home-index--productionPhotos {
  width: 30em;
}

.home-index-sectionHeads {
  padding-top: 2em;
  font-family: "oswald-variablefont_wght";
}

.home-index-sectionHeads:after {
  background: none repeat scroll 0 0 var(--main-color);
  bottom: -10px;
  margin-bottom: 1em;
  content: "";
  display: block;
  height: 5px;
  position: relative;
  width: 100px;
}

.home-index--NAYAimage {
  padding-top: 70em;
  text-align: center;
}
```

## Calendar Events
![Calendar Events GIF](/GIFs/calendarEvents.gif)
### Code
```cs
public class CalendarEventModel
    {
        [Key] public int EventId { get; set; }

        public string Title { get; set; }

        [DataType(DataType.Date)]
        [Display(Name = "StartDate")]
        public DateTime StartDate { get; set; }

        [DataType(DataType.Date)]
        [Display(Name = "EndDate")]
        public DateTime EndDate { get; set; }

        [DataType(DataType.Time)]
        [Display(Name = "StartTime")]
        public DateTime? StartTime { get; set; }

        [DataType(DataType.Time)]
        [Display(Name = "EndTime")]
        public DateTime? EndTime { get; set; }

        public bool AllDay { get; set; }
        public int TicketsAvailable { get; set; }
        public bool IsProduction { get; set; }

        public virtual Production Production { get; set; }
    }
 ```

## Rental Requests
![Rental Requests GIF](/GIFs/rentalRequests.gif)
### Code
```cshtml
<div class="rent-index--toggleContainer">
  <input class="rent-index--toggleBtn" type="button" id="expiredBtn" value="Expired Requests" />
</div>

<div class="rent-index--accordionContainer">
  <div class="accordion" id="accordionExample">
    @foreach (var item in Model.OrderBy(item => item.StartTime.Date).ThenBy(item => item.StartTime.TimeOfDay))
    {
      DateTime currentDate = DateTime.Now;

      string startTimeString = Html.DisplayFor(modelItem => item.StartTime).ToString();
      DateTime startTime = Convert.ToDateTime(startTimeString);
      TimeSpan diff = startTime.Subtract(currentDate);

      string endTimeString = Html.DisplayFor(modelItem => item.EndTime).ToString();
      DateTime endTime = Convert.ToDateTime(endTimeString);
      TimeSpan remainingDiff = endTime.Subtract(currentDate);

      if (endTime.AddDays(7) > currentDate)
      {
        <div class="rent-index--current card">
          <div class="card-header" id="headingOne">
            <h5 class="mb-0">
              <button class="btn btn-link rent-index--cardBtn" type="button" data-toggle="collapse" data-target="#info-@item.RentalRequestID" aria-expanded="true" aria-controls="collapseOne">
                <div class="rent-index--headBox">
                  <div class="rent-index--headDiv">
                    <p class="rent-index--company">Company: @Html.DisplayFor(modelItem => item.Company)</p>
                    <p class="rent-index--contact">Contact Person: @Html.DisplayFor(modelItem => item.ContactPerson)</p>
                  </div>
                  @if (diff.Days >= 1)
                  {
                    <p class="rent-index--timeUntil">Time Until Start: @diff.Days Days</p>
                  }
                  else if (diff.Days < 1 && diff.Hours >= 1)
                  {
                    <p class="rent-index--timeUntil">Time Until Start: @diff.Hours Hours</p>
                  }
                  else if (diff.Days < 1 && diff.Hours < 1 && diff.Minutes >= 1)
                  {
                    <p class="rent-index--timeUntil">Time Until Start: @diff.Minutes Minutes</p>
                  }
                  else if (remainingDiff.Days >= 1)
                  {
                    <p class="rent-index--timeUntil">Time Left: @remainingDiff.Days Days</p>
                  }
                  else if (remainingDiff.Days < 1 && remainingDiff.Hours >= 1)
                  {
                    <p class="rent-index--timeUntil">Time Left: @remainingDiff.Hours Hours</p>
                  }
                  else if (remainingDiff.Days < 1 && remainingDiff.Hours < 1 && remainingDiff.Minutes >= 1)
                  {
                    <p class="rent-index--timeUntil">Time Left: @remainingDiff.Minutes Minutes</p>
                  }
                  else if (remainingDiff.Days < 1 && remainingDiff.Hours < 1 && remainingDiff.Minutes < 1 && endTime.AddDays(7) > currentDate)
                  {
                    <p class="rent-index--timeUntil">Time Left: None</p>
                  }
                </div>
              </button>
            </h5>
          </div>

          <div id="info-@item.RentalRequestID" class="collapse" aria-labelledby="headingOne" data-parent="#accordionExample">
            <div class="card-body">
              <div class="rent-index--timeBox">
                <p class="rent-index--startTime">Start Time:<br /> <span class="rent-index--dateTime">@startTime.ToShortTimeString() <br /> @startTime.ToString("MMM") @startTime.Day, @startTime.Year</span></p>
                <p class="rent-index--endTime">End Time:<br /> <span class="rent-index--dateTime">@endTime.ToShortTimeString() <br /> @endTime.ToString("MMM") @endTime.Day, @endTime.Year</span></p>
              </div>
              <div class="rent-index--infoBox">
                <p class="rent-index--info">Rental Code: @Html.DisplayFor(modelItem => item.RentalCode)</p>
                <p class="rent-index--info">Project Info: @Html.DisplayFor(modelItem => item.ProjectInfo)</p>
                <p class="rent-index--info">Accepted: @Html.DisplayFor(modelItem => item.Accepted)</p>
                <p class="rent-index--info">Contract Signed: @Html.DisplayFor(modelItem => item.ContractSigned)</p>

                @Html.ActionLink("Edit", "Edit", new { id = item.RentalRequestID }) |
                @Html.ActionLink("Details", "Details", new { id = item.RentalRequestID }) |
                @Html.ActionLink("Delete", "Delete", new { id = item.RentalRequestID })
              </div>
            </div>
          </div>
        </div>
      }
      else
      {
        <div class="rent-index--expired card" >
          <div class="card-header" id="headingOne">
            <h5 class="mb-0">
              <button class="btn btn-link rent-index--cardBtn" type="button" data-toggle="collapse" data-target="#info-@item.RentalRequestID" aria-expanded="true" aria-controls="collapseOne">
                <div class="rent-index--headBox">
                  <div class="rent-index--headDiv">
                    <p class="rent-index--company">Company: @Html.DisplayFor(modelItem => item.Company)</p>
                    <p class="rent-index--contact">Contact Person: @Html.DisplayFor(modelItem => item.ContactPerson)</p>
                  </div>
                  @if (diff.Days >= 1)
                  {
                    <p class="rent-index--timeUntil">Time Until Start: @diff.Days Days</p>
                  }
                  else if (diff.Days < 1 && diff.Hours >= 1)
                  {
                    <p class="rent-index--timeUntil">Time Until Start: @diff.Hours Hours</p>
                  }
                  else if (diff.Days < 1 && diff.Hours < 1 && diff.Minutes >= 1)
                  {
                    <p class="rent-index--timeUntil">Time Until Start: @diff.Minutes Minutes</p>
                  }
                  else if (remainingDiff.Days >= 1)
                  {
                    <p class="rent-index--timeUntil">Time Left: @remainingDiff.Days Days</p>
                  }
                  else if (remainingDiff.Days < 1 && remainingDiff.Hours >= 1)
                  {
                    <p class="rent-index--timeUntil">Time Left: @remainingDiff.Hours Hours</p>
                  }
                  else if (remainingDiff.Days < 1 && remainingDiff.Hours < 1 && remainingDiff.Minutes >= 1)
                  {
                    <p class="rent-index--timeUntil">Time Left: @remainingDiff.Minutes Minutes</p>
                  }
                  else if (remainingDiff.Days < 1 && remainingDiff.Hours < 1 && remainingDiff.Minutes < 1 && endTime.AddDays(7) > currentDate)
                  {
                    <p class="rent-index--timeUntil">Time Left: None</p>
                  }
                  else if (endTime.AddDays(7) <= currentDate)
                  {
                    <p class="rent-index--timeUntil">Expired</p>
                  }
                </div>
              </button>
            </h5>
          </div>

          <div id="info-@item.RentalRequestID" class="collapse" aria-labelledby="headingOne" data-parent="#accordionExample">
            <div class="card-body">
              <div class="rent-index--timeBox">
                <p class="rent-index--startTime">Start Time:<br /> <span class="rent-index--dateTime">@startTime.ToShortTimeString() <br /> @startTime.ToString("MMM") @startTime.Day, @startTime.Year</span></p>
                <p class="rent-index--endTime">End Time:<br /> <span class="rent-index--dateTime">@endTime.ToShortTimeString() <br /> @endTime.ToString("MMM") @endTime.Day, @endTime.Year</span></p>
              </div>
              <div class="rent-index--infoBox">
                <p class="rent-index--info">Rental Code: @Html.DisplayFor(modelItem => item.RentalCode)</p>
                <p class="rent-index--info">Project Info: @Html.DisplayFor(modelItem => item.ProjectInfo)</p>
                <p class="rent-index--info">Accepted: @Html.DisplayFor(modelItem => item.Accepted)</p>
                <p class="rent-index--info">Contract Signed: @Html.DisplayFor(modelItem => item.ContractSigned)</p>

                @Html.ActionLink("Edit", "Edit", new { id = item.RentalRequestID }) |
                @Html.ActionLink("Details", "Details", new { id = item.RentalRequestID }) |
                @Html.ActionLink("Delete", "Delete", new { id = item.RentalRequestID })
              </div>
            </div>
          </div>
        </div>
      }
    }
  </div>
</div>
 ```

## Expired Requests
![Expired Requests GIF](/GIFs/expiredRequests.gif)
### Code
```js
$("#expiredBtn").click(function () {

    if ($(".rent-index--current").is(":visible")) {
        $(".rent-index--current").hide();
        $(".rent-index--expired").show();
        $("#expiredBtn").val("Current Requests");
    }
    else if ($(".rent-index--expired").is(":visible")) {
        $(".rent-index--expired").hide();
        $(".rent-index--current").show();
        $("#expiredBtn").val("Expired Requests");
    }
})
 ```
