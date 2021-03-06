---
layout: bidder
title: AppNexus AST (legacy)
description: Prebid AppNexus AST Bidder Adaptor
top_nav_section: dev_docs
nav_section: reference
hide: true
biddercode: appnexusAst
biddercode_longer_than_12: false
prebid_1_0_supported : false
media_types: banner, native, video
---

{: .alert.alert-warning :}
Prebid.js 0.x includes two AppNexus Adaptors: `appnexus` and `appnexusAst`. As part of the transition to Prebid 1.0, the AppNexus AST adapter will become the standard (and only) AppNexus adapter (and be renamed to "AppNexus").


### Bid Params

{: .table .table-bordered .table-striped }
| Name                | Scope    | Description                                                                                                                                                                   | Example                                               | Type      |
|---------------------|----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------|-----------|
| `placementId`       | required | The placement ID from AppNexus.  You may identify a placement using the `invCode` and `member` instead of a placement ID.                                                     | `'234234'`                                            | `string`  |
| `allowSmallerSizes` | optional | If `true`, ads smaller than the values in your ad unit's `sizes` array will be allowed to serve. Defaults to `false`.                                                         | `true`                                                | `boolean` |
| `keywords`          | optional | A set of key-value pairs applied to all ad slots on the page.  Mapped to [query string segments for buy-side targeting](https://wiki.appnexus.com/x/7oCzAQ) (login required). | `{ genre: ['rock', 'pop'] }`                          | `object`  |
| `video`             | optional | Video targeting parameters.  See the [video section below](#appnexus-ast-video) for details.                                                                                  | `{ playback_method: ['auto_play_sound_off'] }`        | `object`  |
| `invCode`           | optional | The inventory code from AppNexus. Must be used with `member`.                                                                                                                 | `'abc123'`                                            | `string`  |
| `member`            | optional | The member ID  from AppNexus. Must be used with `invCode`.                                                                                                                    | `'12345'`                                             | `string`  |
| `reserve`           | optional | Sets a floor price for the bid that is returned. If floors have been configured in the AppNexus Console, those settings will override what is configured here.                | `0.90`                                                | `float`   |

(Sizes set in `adUnit` object will also apply to the AppNexus bid requests.)

{: .alert.alert-danger :}
All AppNexus placements included in a single call to `requestBids` must belong to the same publisher object.  If placements from two different publishers are included in the call, the AppNexus bidder may not return any demand for those placements. <br />
*Note: This requirement does not apply to adapters that are [aliasing]({{site.baseurl}}/dev-docs/publisher-api-reference.html#module_pbjs.aliasBidder) the AppNexus adapter.*

<a name="appnexus-ast-video"></a>

#### video parameters

The following video parameters are supported.  For more information, see the video parameters in the [OpenRTB specification](http://www.iab.com/wp-content/uploads/2016/01/OpenRTB-API-Specification-Version-2-4-DRAFT.pdf).

{: .table .table-bordered .table-striped }
| Name              | Description                                                                                                                                                                                                                                  | Type             |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------|
| `mimes`           | Array of strings listing the content MIME types supported, e.g., `["video/x-flv", "video/x-ms-wmv"]`.                                                                                                                                        | `Array<string>`  |
| `minduration`     | Integer that defines the minimum video ad duration in seconds.                                                                                                                                                                               | `integer`        |
| `maxduration`     | Integer that defines the maximum video ad duration in seconds.                                                                                                                                                                               | `integer`        |
| `startdelay`      | Integer that determines whether to show the ad before, during, or after video content.  If > 0, position is mid-roll and value indicates start delay, in seconds. Allowed values: Pre-roll: `0` (default); Mid-roll: `-1` ; Post-roll: `-2`. | `integer`        |
| `skippable`       | Boolean which, if `true`, means the user can click a button to skip the video ad.  Defaults to `false`.                                                                                                                                      | `boolean`        |
| `playback_method` | Array of strings listing playback methods supported by the publisher.  Allowed values: `"auto_play_sound_on"`; `"auto_play_sound_off"`; `"click_to_play"`; `"mouseover"`; `"auto_play_sound_unknown"`.                                       | `Array<string>`  |
| `frameworks`      | Array of integers listing API frameworks supported by the publisher. Allowed values: None: `0`; VPAID 1.0: `1`; VPAID 2.0: `2`; MRAID 1.0: `3`; ORMMA: `4`; MRAID 2.0: `5`.                                                                  | `Array<integer>` |

### Advantages of using the Appnexus AST (legacy) Adaptor over the AppNexus (legacy) Adaptor

- Requests all the adUnits at once, which reduces latency on page

- Supports additional formats (such as Video and Native)

- Uses the same endpoints and parameter settings as the Prebid 1.x AppNexus adaptor.
