# 164 Multiple Inheritance

<div class="video-wrapper">
  <iframe src="https://www.youtube.com/embed/15yc7XQex-4?si=Jj2LrIb_VmguN1GP"
          title="YouTube video player" 
          frameborder="0" 
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" 
          allowfullscreen>
  </iframe>
</div>

## 1. Concept Introduction

* In C++, a class can inherit from **multiple base classes**.
* Example in the STL:

  * `iostream` inherits from both `istream` and `ostream`.
  * That’s why it supports both input and output operations.

👉 Multiple inheritance gives flexibility, but it also introduces **ambiguity and complexity** if misused.

---

## 2. Step-by-Step Walkthrough with Code

### Part 1 – Basic Multiple Inheritance

```cpp
class FileReader {
public:
    string read() { return "Hello World"; }
};

class FileWriter {
public:
    void write(string content) { cout << content; }
};

class FileIO : public FileReader, public FileWriter {};
```

Usage:

```cpp
FileIO file;
string content = file.read();  // From FileReader
file.write("Hello World");     // From FileWriter
```

✅ A `FileIO` object has members of both `FileReader` and `FileWriter`.

---

### Part 2 – Constructors in Base Classes

* When base classes require constructor arguments, the compiler **cannot auto-generate** a default constructor for the derived class.
* You must explicitly call base constructors:

```cpp
class FileIO : public FileReader, public FileWriter {
public:
    FileIO(string fileName) : FileReader(fileName), FileWriter(fileName) {}
};
```

Usage:

```cpp
FileIO file{"data.txt"};
```

---

### Part 3 – Ambiguity with Same Member Names

If both base classes define the same member:

```cpp
class FileReader {
public:
    string fileName() { return "reader.txt"; }
};

class FileWriter {
public:
    string fileName() { return "writer.txt"; }
};
```

Now:

```cpp
FileIO file{"myfile"};
file.fileName(); // ❌ Compilation error (ambiguous)
```

Fix: explicitly qualify the base class:

```cpp
file.FileReader::fileName();  
file.FileWriter::fileName();
```

---

## 3. Visual Mental Model

```
        FileReader   FileWriter
             \        /
              \      /
               \    /
                 FileIO
```

* FileIO inherits **both capabilities**.
* If overlapping members exist → ambiguity must be resolved manually.

---

## 4. Best Practices & Insights

* ✅ Keep multiple inheritance **shallow and clear**.
* ✅ Only use it when modeling entities that truly combine **independent capabilities**.
* 🚫 Avoid if base classes share too many member names → leads to ambiguity.
* 🚫 Don’t use multiple inheritance just for “common code sharing” → prefer **composition**.

---

## 5. Interview Insights

**Q:** What is multiple inheritance in C++?
**A:** It allows a class to inherit from more than one base class.

**Q:** What problem does it introduce?
**A:** Ambiguity if base classes define members with the same name.

**Q:** How to resolve ambiguity?
**A:** Explicitly qualify with the base class name (`BaseClass::member`).

**Q:** When is it appropriate to use?
**A:** When combining **orthogonal capabilities** (e.g., `Readable`, `Writable`).

---

## 6. Exercise – DateTime Example

We built:

* `Date` (immutable, validated day/month/year)
* `Time` (immutable, validated hour/minute/second)
* `DateTime` (inherits from both `Date` and `Time`).

### Date.h 

```cpp
#ifndef ADVANCED_DATE_H
#define ADVANCED_DATE_H

class Date {
public:
    Date(int date, int month, int year);

    int getDate() const;
    int getMonth() const;
    int getYear() const;

private:
    int date;
    int month;
    int year;
};

#endif
```

### Date.cpp

```cpp
#include "Date.h"

#include <limits>
#include <stdexcept>

Date::Date(int date, int month, int year) {
    if ((date < 1 || date > 31) ||
        (month < 1 || month > 12) ||
        (year < 0 || year > numeric_limits<int>::max())) {
            throw invalid_argument("Either date or month or year is invalid");
        }

    this->date = date;
    this->month = month;
    this->year = year;
}

int Date::getDate() const {
    return date;
}

int Date::getMonth() const {
    return month;
}

int Date::getYear() const {
    return year;
}
```

### Time.h

```cpp
#ifndef ADVANCED_TIME_H
#define ADVANCED_TIME_H

class Time {
public:
    Time(int hour, int minute, int second);

    int getHour() const;
    int getMinute() const;
    int getSecond() const;

private:
    int hour;
    int minute;
    int second;
};

#endif
```

### Time.cpp

```cpp
#include "Time.h"

#include <stdexcept>

Time::Time(int hour, int minute, int second) {
    if ((hour < 0 || hour > 23) ||
        (minute < 0 || minute > 59) ||
        (second < 0 || second > 59)) {
            throw invalid_argument("Either hour or minute or second is Invalid");
        }

    this->hour = hour;
    this->minute = minute;
    this->second = second;
}

int Time::getHour() const {
    return hour;
}

int Time::getMinute() const {
    return minute;
}

int Time::getSecond() const {
    return second;
}
```

### DateTime.h

```cpp
#ifndef ADVANCED_DATETIME_H
#define ADVANCED_DATETIME_H

#include "Date.h"
#include "Time.h"

class DateTime : public Date, public Time {
public:
    DateTime(int date, int month, int year, int hour, int minute, int second);
};

#endif
```

### DateTime.cpp

```cpp
#include "DateTime.h"

DateTime::DateTime(int date, int month, int year, int hour, int minute, int second) : 
    Date(date, month, year), 
    Time(hour, minute, second) {}
```

### main.cpp

```cpp
#include "DateTime.h"
#include <iostream>

using namespace std;

int main() {
    DateTime dt(10, 9, 2025, 10, 30, 45);

    cout 
        << dt.getDate() << "/" 
        << dt.getMonth() << "/" 
        << dt.getYear() << " " 
        << dt.getHour() << ":" 
        << dt.getMinute() << ":" 
        << dt.getSecond() << "\n";

        return 0;
}
```

Output:

```
10/9/2025 10:30:45
```

👉 Practical use-case of multiple inheritance: combining independent domains (`Date` + `Time`).

---

## 7. Quick Revision Sheet

* Multiple inheritance = class inherits from more than one base class.
* Compiler won’t auto-generate constructors if base constructors require args → must delegate manually.
* Ambiguity arises if base classes have methods with the same name → fix with `BaseClass::method()`.
* Great for combining orthogonal features, bad for building deep tangled hierarchies.

---

## 8. Code from the video

### Part 1

#### FileReader.h

```cpp
#ifndef ADVANCED_FILEREADER_H
#define ADVANCED_FILEREADER_H

#include <string>

using namespace std;

class FileReader {
public:
    string read() {
        return "Hello World";
    }
};

#endif
```

#### FileWriter.h

```cpp
#ifndef ADVANCED_FILEWRITER_H
#define ADVANCED_FILEWRITER_H

#include <iostream>
#include <string>

using namespace std;

class FileWriter {
public:
    void write(string content) {
        cout << content;
    }
};

#endif
```

#### FileIO.h

```cpp
#ifndef ADVANCED_FILEIO_H
#define ADVANCED_FILEIO_H

#include "FileReader.h"
#include "FileWriter.h"

#include <string>

using namespace std;

class FileIO : public FileReader, public FileWriter {};

#endif
```

#### main.cpp

```cpp
#include "FileIO.h"

#include <iostream>

using namespace std;

int main() {
    FileIO file;

    // We can access the function of both the base classes
    string content = file.read();
    file.write("Hello World");

    return 0;
}
```

### Part 2

#### FileReader.h

```cpp
#ifndef ADVANCED_FILEREADER_H
#define ADVANCED_FILEREADER_H

#include <iostream>
#include <string>

using namespace std;

class FileReader {
public:
    FileReader(string fileName) {
        cout << "Constructor of FileReader" << endl;
    }

    string read() {
        return "Hello World";
    }
};

#endif
```

#### FileWriter.h

```cpp
#ifndef ADVANCED_FILEWRITER_H
#define ADVANCED_FILEWRITER_H

#include <iostream>
#include <string>

using namespace std;

class FileWriter {
public:
    FileWriter(string fileName) {
        cout << "Constructor of FileWriter" << endl;
    }
    
    void write(string content) {
        cout << content;
    }
};

#endif
```

#### FileReader.h

```cpp
#ifndef ADVANCED_FILEIO_H
#define ADVANCED_FILEIO_H

#include "FileReader.h"
#include "FileWriter.h"

#include <string>

using namespace std;

class FileIO : public FileReader, public FileWriter {};

#endif
```

#### main.cpp

```cpp
#include "FileIO.h"

#include <iostream>

using namespace std;

int main() {
    FileIO file; // Compilation error

    return 0;
}
```

### Part 3

#### FileReader.h

```cpp
#ifndef ADVANCED_FILEREADER_H
#define ADVANCED_FILEREADER_H

#include <iostream>
#include <string>

using namespace std;

class FileReader {
public:
    FileReader(string fileName) {
        cout << "Constructor of FileReader" << endl;
    }

    string read() {
        return "Hello World";
    }
};

#endif
```

#### FileWriter.h

```cpp
#ifndef ADVANCED_FILEWRITER_H
#define ADVANCED_FILEWRITER_H

#include <iostream>
#include <string>

using namespace std;

class FileWriter {
public:
    FileWriter(string fileName) {
        cout << "Constructor of FileWriter" << endl;
    }
    
    void write(string content) {
        cout << content;
    }
};

#endif
```

#### FileReader.h

```cpp
#ifndef ADVANCED_FILEIO_H
#define ADVANCED_FILEIO_H

#include "FileReader.h"
#include "FileWriter.h"

#include <string>

using namespace std;

class FileIO : public FileReader, public FileWriter {
public:
    FileIO(string fileName) : FileReader(fileName), FileWrite(fileName) {}
};

#endif
```

#### main.cpp

```cpp
#include "FileIO.h"

#include <iostream>

using namespace std;

int main() {
    FileIO file{"fileName"};

    return 0;
}
```

### Part 4

#### FileReader.h

```cpp
#ifndef ADVANCED_FILEREADER_H
#define ADVANCED_FILEREADER_H

#include <iostream>
#include <string>

using namespace std;

class FileReader {
public:
    FileReader(string fileName) {
        cout << "Constructor of FileReader" << endl;
    }

    string fileName() {
        return "filename";
    }

    string read() {
        return "Hello World";
    }
};

#endif
```

#### FileWriter.h

```cpp
#ifndef ADVANCED_FILEWRITER_H
#define ADVANCED_FILEWRITER_H

#include <iostream>
#include <string>

using namespace std;

class FileWriter {
public:
    FileWriter(string fileName) {
        cout << "Constructor of FileWriter" << endl;
    }

    string fileName() {
        return "filename";
    }
    
    void write(string content) {
        cout << content;
    }
};

#endif
```

#### FileReader.h

```cpp
#ifndef ADVANCED_FILEIO_H
#define ADVANCED_FILEIO_H

#include "FileReader.h"
#include "FileWriter.h"

#include <string>

using namespace std;

class FileIO : public FileReader, public FileWriter {
public:
    FileIO(string fileName) : FileReader(fileName), FileWrite(fileName) {}
};

#endif
```

#### main.cpp

```cpp
#include "FileIO.h"

#include <iostream>

using namespace std;

int main() {
    FileIO file{"fileName"};
    
    // Compilation error
    // file.fileName() 

    file.FileReader::fileName();

    return 0;
}
```

## 9. Exercise

### Date.h 

```cpp
#ifndef ADVANCED_DATE_H
#define ADVANCED_DATE_H

class Date {
public:
    Date(int date, int month, int year);

    int getDate() const;
    int getMonth() const;
    int getYear() const;

private:
    int date;
    int month;
    int year;
};

#endif
```

### Date.cpp

```cpp
#include "Date.h"

#include <limits>
#include <stdexcept>

Date::Date(int date, int month, int year) {
    if ((date < 1 || date > 31) ||
        (month < 1 || month > 12) ||
        (year < 0 || year > numeric_limits<int>::max())) {
            throw invalid_argument("Either date or month or year is invalid");
        }

    this->date = date;
    this->month = month;
    this->year = year;
}

int Date::getDate() const {
    return date;
}

int Date::getMonth() const {
    return month;
}

int Date::getYear() const {
    return year;
}
```

### Time.h

```cpp
#ifndef ADVANCED_TIME_H
#define ADVANCED_TIME_H

class Time {
public:
    Time(int hour, int minute, int second);

    int getHour() const;
    int getMinute() const;
    int getSecond() const;

private:
    int hour;
    int minute;
    int second;
};

#endif
```

### Time.cpp

```cpp
#include "Time.h"

#include <stdexcept>

Time::Time(int hour, int minute, int second) {
    if ((hour < 0 || hour > 23) ||
        (minute < 0 || minute > 59) ||
        (second < 0 || second > 59)) {
            throw invalid_argument("Either hour or minute or second is Invalid");
        }

    this->hour = hour;
    this->minute = minute;
    this->second = second;
}

int Time::getHour() const {
    return hour;
}

int Time::getMinute() const {
    return minute;
}

int Time::getSecond() const {
    return second;
}
```

### DateTime.h

```cpp
#ifndef ADVANCED_DATETIME_H
#define ADVANCED_DATETIME_H

#include "Date.h"
#include "Time.h"

class DateTime : public Date, public Time {
public:
    DateTime(int date, int month, int year, int hour, int minute, int second);
};

#endif
```

### DateTime.cpp

```cpp
#include "DateTime.h"

DateTime::DateTime(int date, int month, int year, int hour, int minute, int second) : 
    Date(date, month, year), 
    Time(hour, minute, second) {}
```

### main.cpp

```cpp
#include "DateTime.h"
#include <iostream>

using namespace std;

int main() {
    DateTime dt(10, 9, 2025, 10, 30, 45);

    cout 
        << dt.getDate() << "/" 
        << dt.getMonth() << "/" 
        << dt.getYear() << " " 
        << dt.getHour() << ":" 
        << dt.getMinute() << ":" 
        << dt.getSecond() << "\n";

        return 0;
}
```