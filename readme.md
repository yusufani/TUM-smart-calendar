![img](logo.png)
I know that there any many student taking some courses for collecting and archiving necessary documents

So, if you are controlling your life with google calendar and want to see only some of courses in your calendar, 
you are in the right place.

In this repo I will guide you to customizing your TUM Calendar.


Firstly, you need to your Calendar ics link. You can find it like https://wiki.tum.de/display/docs/Personal+Calendar

Then paste your calendar url to https://cal.bruck.me to get rid of all unnecessary titles. 

Now you can subscribe your TUM calendar to your Google calendar. When you subscribe, your calendar automatically will be updated. But the problem is you can not delete or hide events from subscription, rigth? ( Maybe there is a way but I don't know it)

That's way we will also add our ics file to our Google calendar. You can download it if you paste to URL bar. 
ICS file contains events, but they are not updated automatically.

So we have 2 problem :
1. We need to delete some courses from our calendar
2. Also we need to update our calendar automatically

Lets start with first problem.
**Google allow us to manage our calendar with js coding from here : https://script.google.com/home**
_You can look this link to how to use https://rishidhar.me/how-to-bulk-delete-google-calendar-events-via-google-apps-script/_

If you run bellow code with your calendar id, you can delete all unwanted courses easily :D
You can add your courses names in list also you can define allowed date like in the example. Einführung in Quantum Computing will deleted except for wednesday lecture :D
```
//['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat']
function getUnwantedCourses() {
  return  [
  'Master Seminar 3D Vision (IN2107, IN4911)',
  '821093429 Protein Prediction I for Computer Scientists',
  'EXs for Protein Prediction I for Computer Scientists',
  '821093429 Protein Prediction I for Computer Scientists',
  'Master Seminar 3D Vision (IN2107, IN4911) ',
  'Master Seminar D Vision (IN, IN) ,'
  //'ML',
  //'Grundlegende Mathematische Methoden für Imaging und Visualisierung',
  //'Cloud-Based Data Processing',
  //'3D Scanning & Motion Capture' ,
  //'Fragestunde: ML' , {'course' : 'Einführung in Quantum Computing', 'Allowed' :  [3.0, 4.0]  } ,
  //'I to Surgical Robotics'  ,
  //'Computer Vision III: Detektion, Segmentierung und Tracking (Prüfung)' ,
  //'Computer Vision II: Multiple View Geometry' , 
  //'821093429 Protein Prediction I for Computer Scientists' ,
  //'821093429 Protein Prediction I for Computer Scientists' , 
  //'Advanced Natural Language Processing (CIT4230002) ,' , 
  //'EX for Protein Prediction I for Computer Scientists' , 
  //'Deutsch als Fremdsprache A1.2 , Meuschel (MUC Präsenz)' , 
  //'Protein Prediction I for Computer Scientists' , 
  //'EXs for Protein Prediction I for Computer Scientists',
  //'Deutsch als Fremdsprache A1.2 , Menck-Zwick MO',
  //'Deutsch als Fremdsprache A1.2 , Kummer-Rock (MUC Präsenz)',
  //'Deutsch als Fremdsprache A1.2 , Thiessen (MUC Präsenz)',
  //'Master Seminar: 3D Machine Learning (IN2107, IN4429) ,',
  //'Master-Praktikum - Lernbasierte Ansätze für autonome Fahrzeuge und intelligente Systeme',
  //'Master-Praktikum - Advanced Topics in 3D Computer Vision',
  //'Deutsch als Fremdsprache A1.2 , Reulein (MUC Präsenz/Online)',
  //'Natural Language Processing - Verfahren und Anwendungen (IN2107, IN4816) ,',
  //'Master-Seminar - Diffusion Modelle zur Bildgenerierung (IN2107, IN45005) ,',
  //'Master-Seminar - Diffusion Modelle zur Bildgenerierung (IN2107, INXXXX)',
  //'Master-Seminar - Diffusion Modelle zur Bildgenerierung (IN2107, INXXXX) ,',
  //'Deutsch als Fremdsprache A1.2 , Pinskaia DO (MUC Präsenz)',
  //'Deutsch als Fremdsprache A1.2 , Khvintelani',
  //'Master Lab Course - Ethical AI: Problems and Applications',
  //'Master Lab Course – Ethical AI: Problems and Applications',
  //'Deutsch als Fremdsprache A1.2 , Lechle (GAR Präsenz)',
  //'Master-Seminar - Diffusion Modelle zur Bildgenerierung (IN2107, INXXXX)',
  //'Deutsch als Fremdsprache A1.2 , Zerfass',
  //'Deutsch als Fremdsprache A1.2 , Schlüter (MUC Hybrid/Online)',
  //'Master-Seminar: Interpretable AI in Medical Imaging (IN2107, IN45009) ,',
  //'Master-Seminar: Interpretable AI in Medical Imaging',
  //'Master Seminar 3D Vision (IN2107, IN4911) ,',
  //'Deutsch als Fremdsprache A1.2 , Menck-Zwick MI',
  //'Deutsch als Fremdsprache A1.2 , Hanke (GAR Präsenz)',
  //'Deutsch als Fremdsprache A1.2 , Jennert (MUC Präsenz)',
  //'Deutsch als Fremdsprache A1.2 , Pinskaia MO (MUC Präsenz)',
  //'Seminar - Recent Trends in 3D Computer Vision',
  //'Seminar - Recent Trends in 3D Computer Vision (IN0014, IN2107, IN4826) ,',
  //'Entwicklungspraktikum NLP-based Software Engineering (IN2106, IN2129) ,,',
  //'Entwicklungspraktikum NLP-based Software Engineering',
  //'Master-Praktikum - Geometrische Szenenanalyse (IN2106, INXXXX)',
  ];
}

function logAndDeleteEvent(ev) {
  Logger.log('Deleting event: ' + ev.getTitle() + ' on ' + ev.getStartTime());
  ev.deleteEvent();
}

function logAndKeepEvent(ev) {
  Logger.log('Keeping event: ' + ev.getTitle() + ' on ' + ev.getStartTime());
}

function deleteUnnecessaryCourses() {
  const unwantedCourses = getUnwantedCourses();
  const fromDate = new Date(2023, 3, 1);
  const toDate = new Date(2023, 10, 16);
  const calendarId = 'yusufani8@gmail.com';
  const calendar = CalendarApp.getCalendarById(calendarId);

  unwantedCourses.forEach(course => {
    let allowedDays = [];
    if (typeof course === 'object') {
      allowedDays = course.Allowed;
      course = course.course;
    }
    const events = calendar.getEvents(fromDate, toDate, {search: course});

    events.forEach(ev => {
      if (allowedDays.length && !allowedDays.includes(ev.getStartTime().getDay())) {
        logAndDeleteEvent(ev);
      } else {
        logAndKeepEvent(ev);
      }
    });
  });

  Logger.log('Course deletion process completed.');
}
// delete_unnecessary_courses()


```





For the synchronizing problem, we will use Google script feature again. Iƒ you add your calendar id and subscription calendar id bellow,
this code will sync your calendar with subscription calendar. After edit the code, you can create a trigger like this : 

![img](img.png)

```



function isNumeric(str) {
  return !isNaN(str) && !isNaN(parseFloat(str));
}

function fixCourseName(name) {
  let fixedName = name.replace(/[0-9]/g, '').trim();
  const firstWord = fixedName.split(' ')[0];

  if (firstWord.length === 10 && isNumeric(firstWord[0])) {
    fixedName = fixedName.split(' ').slice(1).join(' ');
  }
  return fixedName.replace('&amp;', '');
}

function getDifference(array1, array2) {
  return array1.filter(event1 => {
    return !array2.some(event2 => {
      return event1.getStartTime().getTime() === event2.getStartTime().getTime() &&
             fixCourseName(event1.getTitle()) === fixCourseName(event2.getTitle());
    });
  });
}

function isWanted(event, unwantedCourses) {
  const eventName = fixCourseName(event.getTitle());

  for (const course of unwantedCourses) {
    let allowedDays = [];
    let courseName = course;

    if (typeof course === 'object') {
      allowedDays = course.Allowed;
      courseName = course.course;
    }

    if (courseName.trim().replace('&', '') === eventName) {
      if (allowedDays.length && !allowedDays.includes(event.getStartTime().getDay())) {
        return false;
      }
    }
  }
  return true;
}

function syncCalendars() {
  const fromDate = new Date(2023, 8, 1);
  const toDate = new Date(2024, 4, 16);
  const mainCalendarId = 'yourmaincalendarid@group.calendar.google.com'; //Enter your final target calendar ID here
  const tumCalendarId = 'yourtumcalendarid@import.calendar.google.com' ; // Subsrice a calendar with cal.bruck url in google calendar and get this


  const mainCalendar = CalendarApp.getCalendarById(mainCalendarId);
  const tumCalendar = CalendarApp.getCalendarById(tumCalendarId);

  const mainEvents = mainCalendar.getEvents(fromDate, toDate);
  const tumEvents = tumCalendar.getEvents(fromDate, toDate);
  Logger.log('Number of tum events' + tumEvents.length)

  const unwantedCourses = getUnwantedCourses();

  const distinctTumEvents = getDifference(tumEvents, mainEvents);
  distinctTumEvents.forEach(ev => {
    if (isWanted(ev, unwantedCourses)) {
      Logger.log('Synchronizing event: ' + ev.getTitle());
      mainCalendar.createEvent(fixCourseName(ev.getTitle()), ev.getStartTime(), ev.getEndTime(), {
        'description': ev.getDescription(),
        'location': ev.getLocation()
      });
    }
  });

  const distinctMainEvents = getDifference(mainEvents, tumEvents);
  distinctMainEvents.forEach(ev => {
    if (!isWanted(ev, unwantedCourses)) {
      Logger.log('Deleting canceled event: ' + ev.getTitle());
      ev.deleteEvent();
    }
  });

  Logger.log('Calendar synchronization completed.');
}

syncCalendars();
```

```
