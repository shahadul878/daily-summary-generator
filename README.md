# Daily Summary Generator

A simple, modern web app to generate daily summaries, copy them, and (optionally) send them to a Google Sheet for record-keeping.

---

## Features
- Add multiple summary entries (Username, Client Name, Website URL, Additional Text)
- Generate a formatted daily summary
- Copy the summary to clipboard
- Optionally send all entries to a connected Google Sheet
- Clear the summary and reset the form with one click
- Responsive, user-friendly UI

---

## Setup & Usage

1. **Download or Clone the Repository**
   - Place all files in a folder and open `index.html` in your browser.

2. **Fill Out the Form**
   - Enter Username, Client Name, Website URL, and Additional Text for each entry.
   - Click "+ Add More" to add more entry groups.

3. **Generate and Copy Summary**
   - Click **Add to Summary** to see the formatted summary.
   - Click **Copy Summary** to copy the summary to your clipboard.

4. **Send to Google Sheet (Optional)**
   - Check the **Send to Google Sheet** box before copying the summary.
   - When you click **Copy Summary**, you’ll be asked to confirm sending the data to your Google Sheet.
   - Each entry will be appended as a new row in your connected Google Sheet.

5. **Clear Summary**
   - Click **Clear Summary** to clear the summary and reset the form.

---

## Google Sheets Integration

- This app uses a Google Form as a backend to append rows to a Google Sheet (no login required).

### How to Connect Your Google Form (Step-by-Step)

#### 1. **Create a Google Form**
- Go to [Google Forms](https://forms.google.com) and create a new form.
- Add these fields (as short answer questions):
  - Username
  - Client Name
  - Website URL
  - Additional Text

#### 2. **Link the Form to a Google Sheet**
- In your Google Form, click the **Responses** tab.
- Click the green Sheets icon to create a linked Google Sheet. All form submissions will appear as new rows in this sheet.

#### 3. **Find Your Form’s Entry IDs**
Each field in your form has a unique `entry.xxxxxxxx` ID. You need these to send data from your app.

**Easiest Method: Pre-filled Link**
1. In the Google Form editor, click the three dots (⋮) in the top right and select **Get pre-filled link**.
2. Enter test values in each field (e.g., "test1", "test2", etc.).
3. Click **Get link** and copy the generated URL.
4. Paste the URL into a text editor. It will look like:
   ```
   https://docs.google.com/forms/d/e/FORM_ID/viewform?usp=pp_url&entry.2041424832=test1&entry.1050326521=test2&entry.2046462741=test3&entry.1581039888=test4
   ```
5. The `entry.xxxxxxxx` parts in the URL are your field IDs. Match them to your field labels.

**Alternative: Inspect the Form Source**
- Open the public form, right-click, and choose **View Page Source**.
- Search for `entry.` and look for lines like `<input name="entry.2041424832" ...>`

#### 4. **Update the App’s AJAX Code**
- In `index.html`, find the section where data is sent to Google Sheets (look for `$.ajax({ ... })`).
- Replace the entry IDs in the data object with your own if they are different.
- Example:
  ```js
  $.ajax({
    url: "https://docs.google.com/forms/d/e/YOUR_FORM_ID/formResponse",
    data: {
      "entry.2041424832": username,
      "entry.1050326521": clientName,
      "entry.2046462741": websiteUrl,
      "entry.1581039888": additionalText
    },
    type: "POST",
    dataType: "xml"
  });
  ```

#### 5. **Set the POST URL**
- The POST URL is your form’s link with `/viewform` replaced by `/formResponse`.
- Example:
  - Form link: `https://docs.google.com/forms/d/e/FORM_ID/viewform`
  - POST URL: `https://docs.google.com/forms/d/e/FORM_ID/formResponse`

#### 6. **Test Your Integration**
- Fill out the form in your app, check "Send to Google Sheet," and copy the summary.
- Check your linked Google Sheet for new rows.

---

**Tips:**
- The order of columns in your Google Sheet matches the order of fields in your form.
- You can add or remove fields, but you must update both your form and the app’s code to match.
- If you have trouble finding entry IDs, use the pre-filled link method for the easiest experience.

---

## Customization
- You can change the summary format, add more fields, or adjust the UI by editing `index.html`.
- For advanced integrations (e.g., authentication, other APIs), further development is required.

---

## License
This project is provided as-is for personal or business use. No warranty is provided. 