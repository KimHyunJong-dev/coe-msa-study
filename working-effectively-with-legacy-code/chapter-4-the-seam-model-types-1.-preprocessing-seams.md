# Chapter 4: The Seam Model Types - 1. Preprocessing Seams

## Chapter 4: The Seam Model Types - 1. Preprocessing Seams

#### Preprocessing Seams

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

* extren함수인 db\_update에 의존성이 있다.

  \`\`\`c

  **include “localdefs.h”**

## ifdef TESTING

… struct DFHLItem \*last\_item = NULL; int last\_account\_no = -1;

## define db\_update\(account\_no,item\)\

```text
{last_item = (item); last_account_no = (account_no);}
```

…

## endif

\`\`\`

* Preprocesser를 이용하여 db\_update 함수를 대체 할 수 있다.

