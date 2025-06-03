# NEET (UG) 2025 Marks Calculator

A simple web application to calculate marks for NEET (UG) 2025 examination based on recorded responses.

## Features

- Calculate marks based on the official answer key
- Support for all test booklet codes (45-48)
- Handles multi-correct questions
- Shows detailed breakdown of correct, incorrect, and blank answers
- Lists all incorrect question numbers

## How to Use

1. Select your test booklet code (45-48)
2. Upload your recorded responses in CSV format
3. Click "Calculate Marks" to see your results

## CSV Format

The input CSV file should have two columns:
- First column: Question number
- Second column: Recorded response (1, 2, 3, 4, or blank)

Example:
```
1,2
2,3
3,-
4,1
```

## Downloading Response CSV File

To download your recorded responses from the NEET OMR page as a CSV file, you can use the following JavaScript code. Copy and paste this into your browser's developer console while on the OMR page:

```javascript
(function() {
  // 1) Locate the OMR table
  const grid = document.querySelector("[id^='ctl00_LoginContent_grOMR']");
  if (!grid) {
    console.error("OMR table not found. Make sure you are on the Challenge OMR page and logged in.");
    return;
  }

  // 2) Collect each question's number and recorded answer
  const rows = Array.from(grid.querySelectorAll("tr"));
  const lines = ["Qno,RecordedResponse"]; // CSV header

  rows.forEach(tr => {
    const qSpan = tr.querySelector("span[id$='_lbl_QuestionNo']");
    const rSpan = tr.querySelector("span[id$='_lbl_RAnswer']");
    if (qSpan && rSpan) {
      const qno = qSpan.textContent.trim().padStart(3, "0");
      // rSpan.textContent is "-" if blank, or a digit if marked.
      const raw = rSpan.textContent.trim();
      // Treat "-" as empty:
      const ans = (raw === "-" ? "" : raw);
      lines.push(`${qno},${ans}`);
    }
  });

  // 3) Create a Blob and download link
  const csvContent = lines.join("\n");
  const blob = new Blob([csvContent], { type: "text/csv" });
  const url = URL.createObjectURL(blob);

  // 4) Create a temporary <a> to trigger download
  const a = document.createElement("a");
  a.href = url;
  a.download = "NEET_OMR_Responses.csv";
  document.body.appendChild(a);
  a.click();
  document.body.removeChild(a);

  // 5) Revoke the object URL after a moment
  setTimeout(() => URL.revokeObjectURL(url), 1000);
})();
```

This script will:
1. Find the OMR table on the page
2. Extract question numbers and recorded responses
3. Generate a CSV file in the correct format
4. Automatically download the file as "NEET_OMR_Responses.csv"

The downloaded CSV file will be compatible with this marks calculator.

## Hosting

This application is hosted on GitHub Pages. You can access it at: [Your GitHub Pages URL will appear here after deployment]

## Local Development

To run this locally:
1. Clone this repository
2. Open `index.html` in your web browser

## License

MIT License 