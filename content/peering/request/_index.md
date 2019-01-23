---
title: "Peering Request"
date: 2019-01-01
draft: false
linktitle: Peering Request
layout: subsection
---
## Request Direct Peering with {{< asn >}}
<br>
<form action="https://formspree.io/peering@as14525.net" method="POST">
<div class="field">
  <div class="field-body">
    <div class="field">
    <div class="field has-addons">
      <p class="control">
        <a class="button is-static">
          Contact Name
        </a>
      </p>
      <p class="control is-expanded">
        <input class="input" type="text" name="Contact Name">
      </p>
    </div>
    </div>
    <div class="field">
    <div class="field is-expanded">
      <div class="field has-addons">
        <p class="control">
          <a class="button is-static">
            Email
          </a>
        </p>
        <p class="control is-expanded">
          <input class="input" type="text" name="Email">
        </p>
      </div>
    </div>
    </div>
  </div>
</div>

<div class="field">
  <div class="field-body">
    <div class="field is-expanded">
      <div class="field has-addons">
        <p class="control">
          <a class="button is-static">
            Organization
          </a>
        </p>
        <p class="control is-expanded">
          <input class="input" type="text" name="Organization">
        </p>
      </div>
    </div>
  </div>
</div>

<div class="field">
  <div class="field-label"></div>
  <div class="field-body">
    <div class="field is-expanded">
      <div class="field has-addons">
        <p class="control">
          <a class="button is-static">
            AS
          </a>
        </p>
        <p class="control is-expanded">
          <input class="input" type="text" placeholder="e.g. 64496" name="ASN">
        </p>
      </div>
    </div>
  </div>
</div>
<div class="label">Facility</div>
  <div class="select is-multiple">
    <select multiple size="5" name="Facility">
      <option value="phx01">US: Phoenix, AZ</option>
      <option value="lsv01">US: Las Vegas, NV</option>
      <option value="hnl01">US: Honolulu, HI</option>
      <option value="day01">US: Dayton, OH</option>
      <option value="nyc01">US: New York, NY</option>
    </select>
  </div>
<br><br>

<div class="field is-grouped">
  <div class="control">
    <p class="help">By submitting this request, you hereby agree to the <a href="peering/policy/">Peering Policy.</a></p>
    <button class="button is-link">Submit</button>
  </div>
</div>
</form>
