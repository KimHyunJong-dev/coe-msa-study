# PART I: The Mechanics of Change
## Chapter 4: The Seam Model Types - 2. Link Seams
### Link Seams
```java
package fitnesse;

import fit.Parse;
import fit.Fixture;
import java.io.*;
import java.util.Date;
import java.io.*;
import java.util.*;

public class FitFilter {
  public String input;
  public Parse tables;
  public Fixture fixture = new Fixture();
  public PrintWriter output;

  public static void main (String argv[]) {
    new FitFilter().run(argv);
  }

  public void run (String argv[]) {
    args(argv);
    process();
    exit();
  }

  public void process() {
    try {
      tables = new Parse(input);
      fixture.doTables(tables);
    } catch (Exception e) {
      exception(e);
    }
    tables.print(output);
  }
  …
}
```
- 외부 라이브러리의 fit.Parse와 fit.Fixture를 사용하였다. 이 경우 Seam은 process 메서드의 new Parse(input)이다. classpath를 변경하여 테스트용으로 작성한 fit.Parse와 fit.Fixture를 호출하도록 변경할 수 있다.
- Link Seam은 환경설정파일 또는 빌드 배포 스크립트에 있을 수 있으며 **테스트 환경과 프로덕션 환경의 다름을 명확히** 하여 혼란을 방지하여야 한다.

[Chapter 4로 이동](https://github.com/SDSACT/microservice-coe-refactoring/wiki/%5BWELC%5D-Chapter-4:-The-Seam-Model)