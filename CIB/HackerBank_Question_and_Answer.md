# React (TypeScript): HackerBank

## Question

Create a banking application as shown below. Some core functionalities have already been implemented, but the application is not complete. Application requirements are given below, and the finished application must pass all of the unit tests.

### Implement the following functionalities:

- All transactions are initially displayed inside the table in the order they are retrieved from the source. The source is passed down as `txns` prop to `TransactionTable` component in `App.tsx`.
- Picking the date from the date input and pressing the 'Filter' button should display all the records for that date in the table. If no date is chosen, the 'Filter' button should not do anything.
- Clicking on the 'Amount ($)' table header should sort the records in ascending order of amount. The behavior is the same for multiple clicks on 'Amount ($)'.

### Each transaction object contains the following properties:

- `String date`: The date when the transaction took place in the format `YYYY-MM-DD`.
- `String description`: The description of the transaction.
- `Number type`: The type of transaction, where `0` denotes a credit transaction and `1` denotes a debit transaction (ignore other types).
- `Float amount`: The total amount of the transaction.
- `String balance`: The balance of the account after the transaction was completed, prefixed with a dollar sign (`$`).

### Example transaction object

```json
{
  "date": "2019-12-03",
  "description": "HACKERBANK INC. DES:CCD+ ID: 33375894749",
  "type": 0,
  "amount": 1985.4,
  "balance": "$12,234.45"
}
```

---

## Answer

The starter `TransactionTable.tsx` only rendered one hardcoded `<tr>`, had an empty `sort` function, and the date input had no `onChange`/filter logic wired up. Below is the completed component.

```tsx
import { useState } from "react";
import { Transaction } from "../types/transaction";

interface TransactionTableProps {
  txns: Transaction[];
}

const TransactionTable: React.FC<TransactionTableProps> = ({ txns }) => {
  const [displayedTxns, setDisplayedTxns] = useState<Transaction[]>(txns);
  const [selectedDate, setSelectedDate] = useState<string>("");

  const handleDateChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    setSelectedDate(e.target.value);
  };

  const handleFilter = () => {
    if (!selectedDate) return;
    setDisplayedTxns(txns.filter((t) => t.date === selectedDate));
  };

  const sort = () => {
    setDisplayedTxns((prev) => [...prev].sort((a, b) => a.amount - b.amount));
  };

  return (
    <div className="layout-column align-items-center mt-50">
      <section className="layout-row align-items-center justify-content-center">
        <label htmlFor="date" className="mr-10">
          Transaction Date
        </label>
        <input
          id="date"
          type="date"
          onChange={handleDateChange}
          value={selectedDate}
          role="search"
        />
        <button onClick={handleFilter}>Filter</button>
      </section>

      <table className="mt-10 table">
        <thead>
          <tr className="table">
            <td className="table-header">Date</td>
            <td className="table-header">Description</td>
            <td className="table-header">Type</td>
            <td className="table-header">
              <span id="amount" onClick={sort} role="button">
                Amount ($)
              </span>
            </td>
            <td className="table-header">Available Balance</td>
          </tr>
        </thead>
        <tbody>
          {displayedTxns.map((txn, i) => (
            <tr key={`${txn.date}-${txn.description}-${i}`}>
              <td>{txn.date}</td>
              <td>{txn.description}</td>
              <td>{txn.type === 1 ? "Debit" : "Credit"}</td>
              <td>{txn.amount}</td>
              <td>{txn.balance}</td>
            </tr>
          ))}
        </tbody>
      </table>
    </div>
  );
};

export default TransactionTable;
```

### Key decisions

- `displayedTxns` state holds whatever's currently shown; the `txns` prop stays the untouched source, so filtering always starts fresh from the full dataset.
- `handleFilter` no-ops if `selectedDate` is empty, matching the "Filter should not do anything with no date chosen" requirement.
- `sort` sorts whatever's currently displayed (respecting an active filter) and always ascending, since repeated clicks should behave the same each time — no toggle logic.
- Switched `defaultValue` → `value` on the date input since a controlled input is needed to read the picked date; mixing `defaultValue` with `onChange` throws a React warning.
