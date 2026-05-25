# CPAgrip Locker — Corrected Setup Guide
# Based on your actual screenshot (image 9)

---

## WHY THE CSS WASN'T APPLYING

The "CSS Tweaks" field on the Desktop tab only styles the **outer overlay/backdrop**.
The white locker box itself is rendered by CPAgrip's own template engine and
cannot be overridden with that CSS field.

The correct way to theme the locker is through the **built-in colour pickers**
on the Desktop tab, **not** through CSS Tweaks.

---

## GENERAL TAB — exact values

**Name:** Survey Blackbook Locker

**Header Text** — click the `<>` source button and paste this EXACTLY:

```html
<div style="text-align:center;font-family:Arial,sans-serif;padding:4px 0 8px">
<div style="font-size:15px;font-weight:700;color:#FFFFFF;margin-bottom:6px">🔒 Your Blackbook Is One Step Away</div>
<div style="font-size:12px;color:#8AAAC8;line-height:1.6">Complete one free offer below — takes under 30 seconds.<br>No credit card. No purchase required.</div>
</div>
```

> Note: Use Arial (not Inter/Sora) — CPAgrip does not load Google Fonts
> in the header field. Arial renders cleanly and is always available.

**Show Offers:** 3
**Access Time:** Forever  ← change from "24 Hours" to "Forever" so returning
                           visitors aren't locked out after 24hrs
**Allow Close:** No
**Allow Scrolling?:** ✓ checked
**Load Method:** On Page Load
**Tease Time:** 0
**Anchor Text:**
```
Your download unlocks immediately after completing one offer.
```

---

## DESKTOP TAB — exact values

**Widget Style:** Rounded
**Hide Borders:** unchecked
**Background Image URL:** leave blank
**Width:** 460
**Height:** auto
**Font Face:** Arial  ← matches what CPAgrip can actually render
**Overlay:** 0.95    ← darker backdrop makes the box stand out more
**Speed:** 400
**Offer Size:** 16

**Colors:**
- `:normal` colour picker → click it → enter hex: `#1A3A6A`
  (this sets offer row background to dark navy blue)
- `:hover` colour picker → click it → enter hex: `#FF6B35`
  (this sets offer row hover to your orange)

**CSS Tweaks field** — paste this. It styles the OVERLAY and whatever
outer elements CPAgrip exposes. It will NOT reach the inner white box
(that is controlled by the colour pickers above):

```css
body { background-color: #050D1C !important; }
.cg-overlay, .locker-overlay, .overlay-bg { background-color: #050D1C !important; }
.cg-locker-wrapper, .locker-wrapper { background: #0F1D35 !important; border: 1px solid #1A3050 !important; border-radius: 10px !important; }
.cg-locker-title, .locker-title { color: #FFFFFF !important; font-family: Arial, sans-serif !important; }
.cg-anchor-text, .anchor-text, .locker-anchor { color: #4A6A8A !important; font-size: 12px !important; font-family: Arial, sans-serif !important; text-align: center !important; }
```

---

## MOBILE TAB

**Enable Mobile Locker:** ✓ checked
**Overlay:** 0.95

**Mobile CSS Tweaks:**
```css
.mobile_header { color: #FFFFFF !important; font-family: Arial, sans-serif !important; font-size: 14px !important; text-align: center !important; padding: 12px !important; }
.mobile_help_text { color: #6A8AA8 !important; font-size: 12px !important; text-align: center !important; }
.mobile_footer { color: #2A3A52 !important; font-size: 11px !important; }
```

**Custom Mobile HTML:** leave blank

---

## ADVANCED TAB

**Locker Actions:** Content Unlock tab selected
**Unlock Action:** Redirect to URL
**Redirect URL:** paste your thankyou.html full URL here
  e.g. `https://your-site.netlify.app/thankyou.html`
**Message:** (leave as is — not shown when redirecting)
**Leads Required:** 1

---

## THE REAL FIX FOR THE WHITE BOX

The white locker popup background comes from CPAgrip's own template.
The only reliable way to make it dark is:

### Option A — Use a Dark Template (Easiest)
1. Click the **Templates** tab in the editor
2. Look for a template labelled "Dark" or "Dark Theme"
3. Select it and click Apply
4. Then re-enter your colour values above (templates reset them)

### Option B — Custom Mobile HTML injection
In the Mobile tab → **Custom Mobile HTML** field, paste this.
It only affects mobile but covers ~70% of your traffic:

```html
<style>
body, .cg-locker, .locker-box, .content-locker,
div[id*="locker"], div[class*="locker"] {
  background-color: #0F1D35 !important;
  color: #8AAAC8 !important;
  font-family: Arial, sans-serif !important;
  border-radius: 10px !important;
}
.cg-locker-title, h2, h3 { color: #FFFFFF !important; }
a { color: #4A6A8A !important; }
.offer-item, .cg-offer, .survey-item {
  background: #0C1A2E !important;
  border: 1px solid #1A3050 !important;
  border-radius: 6px !important;
  color: #B0C8E0 !important;
  margin-bottom: 8px !important;
  padding: 10px 12px !important;
}
</style>
```

### Option C — Inspect the live locker (Most Accurate)
1. Open your landing page in Chrome
2. Trigger the locker (click your CTA button or visit the locker URL directly)
3. Right-click the white box → Inspect
4. Find the actual class names on the white container element
5. Send me a screenshot of the Elements panel
6. I will write CSS that targets those exact classes

Option C gives 100% accurate CSS because we see the real rendered class names.
Without that, all CSS targeting is guesswork against CPAgrip's private template.

---

## WHAT YOUR SCREENSHOT SHOWS IS HAPPENING

From image 9, the locker is showing:
- White background box ✗ (should be #0F1D35)
- Blue link-style text for offers ✗ (should be #B0C8E0)
- "Sorry, there are no offers in your region" message

The "no offers" message is because you are previewing from Bangladesh
and the offers are set to US traffic only. This is NORMAL — the locker
will show real offers when a US visitor loads it. Your traffic from
TikTok will be US-based so this will work correctly in production.

The white box issue can only be definitively fixed with Option C above.

---

## CHECKLIST — what to do right now

- [ ] Change Anchor Text to: "Your download unlocks immediately after completing one offer."
- [ ] Change Access Time to: Forever
- [ ] Set Colors :normal to #1A3A6A and :hover to #FF6B35
- [ ] Change Overlay to 0.95
- [ ] In Advanced tab: set Unlock Action to "Redirect to URL" and paste your thankyou.html URL
- [ ] Try the Templates tab → apply a Dark template if available
- [ ] If white box persists: use Option C (inspect live locker) and send screenshot
