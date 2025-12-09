# Fixing Character Encoding and Time Slot Issues

## Issues Found:
1. ✅ Peso symbols displaying as `â‚±` instead of `₱`
2. ✅ Time validation working but needs testing
3. ❌ Time slot buttons missing hover effect

## Quick Fix Script

Run this in PowerShell (as Administrator):

```powershell
# Navigate to project directory
cd "C:\Users\Administrator\Downloads\dropbar-main\complete-new-databased"

# Fix booking.js encoding
$content = Get-Content "js\booking.js" -Raw -Encoding UTF8
$content = $content.Replace("â‚±", "₱")
$content = $content.Replace("Â·", "·")
$content = $content.Replace("â€"", "–")
$content = $content.Replace("â€¦", "…")
$utf8NoBom = New-Object System.Text.UTF8Encoding $false
[System.IO.File]::WriteAllText("js\booking.js", $content, $utf8NoBom)

Write-Host "✓ Fixed booking.js encoding"

# Fix customer.js
$content = Get-Content "js\customer.js" -Raw -Encoding UTF8
$content = $content.Replace("â‚±", "₱")
$utf8NoBom = New-Object System.Text.UTF8Encoding $false
[System.IO.File]::WriteAllText("js\customer.js", $content, $utf8NoBom)

Write-Host "✓ Fixed customer.js encoding"

Write-Host "`n✅ Done! Clear browser cache and refresh."
```

## What the Time Validation Does:

The time validation I added checks:
- If the selected date is TODAY
- If current time is past the slot's END time
- Example: At 1:25 PM, the "9am-12pm" slot will show "Not Available" (red text)

## Time Slot Hover Effect:

The CSS for time slot hover is in `booking.css`. Make sure you have:

```css
.time-slot {
  transition: all 0.3s ease;
}

.time-slot:hover:not(:disabled) {
  transform: translateY(-2px);
  box-shadow: 0 4px 8px rgba(0,0,0,0.15);
}

.time-slot.selected {
  background: #000;
  color: #fff;
  border-color: #000;
}
```

## Test the Time Validation:

1. Go to booking page
2. Select TODAY's date
3. Look at the time slots:
   - Past slots should show "Not Available" in red
   - Future slots should show available slot count
   - Buttons for past slots should be grayed out and disabled

Current time: **1:25 PM**
- 9am-12pm → ❌ Not Available (past)
- 12pm-3pm → ✅ Available (current/future)
- 3pm-6pm → ✅ Available (future)
