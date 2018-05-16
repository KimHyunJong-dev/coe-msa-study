# PART I: The Mechanics of Change
## Chapter 4: The Seam Model Types - 1. Preprocessing Seams
### Preprocessing Seams
```c
#include <DFHLItem.h>
#include <DHLSRecord.h>
extern int db_update(int, struct DFHLItem *);
void account_update(int account_no, struct DHLSRecord *record, int activated)
{
  if (activated) {
    if (record->dateStamped && record->quantity > MAX_ITEMS) {
      db_update(account_no, record->item);
    } else {
      db_update(account_no, record->backup_item);
    }
  }
  db_update(MASTER_ACCOUNT, record->item);
}
```
- extren함수인 db_update에 의존성이 있다.
```c
#include “localdefs.h”

#ifdef TESTING
…
struct DFHLItem *last_item = NULL;
int last_account_no = -1;
#define db_update(account_no,item)\
    {last_item = (item); last_account_no = (account_no);}
…
#endif
```
- Preprocesser를 이용하여 db_update 함수를 대체 할 수 있다.

[Chapter 4로 이동](https://github.com/SDSACT/microservice-coe-refactoring/wiki/%5BWELC%5D-Chapter-4:-The-Seam-Model)