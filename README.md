# Minecraft Education Edition: User Reviews with Thematic Annotations

This dataset contains **27,036 English-language user reviews** of *Minecraft Education Edition*, collected from the Google Play Store and the Apple App Store and annotated with a fifteen-category thematic codebook.

It accompanies the paper:

> Di Leo, N. *The reception of Minecraft Education Edition in user reviews: an LLM-assisted thematic analysis.* Presented at [WAILS 2026](https://wailsworkshop.github.io/2026/), the 3rd Workshop on Artificial Intelligence with and for Learning Sciences (Paris, 9–11 December 2026).

## Background

Minecraft Education Edition is the classroom version of Minecraft, released by Mojang and Microsoft in 2016. It adds coding tools, lesson content and classroom-management features to Minecraft's building mechanics. The study asks how users actually receive it: whether reviews talk about learning, play, technical problems or access problems, and where the gap between the app's educational promise and user experience appears.

## How the data was built

1. **Collection.** Reviews were collected in July 2026 from the Google Play Store (package `com.mojang.minecraftedu`) and the Apple App Store (ID `1196524622`), using English-language settings and English-oriented storefronts. The Microsoft Store was not included because no extraction tool was available.
2. **Cleaning.** Reviews detected as non-English were removed; reviews too short for reliable language detection were kept. Duplicates were removed. Review dates range from 2018-09-09 to 2026-07-08.
3. **Codebook.** Two annotators developed the codebook inductively through reflexive thematic analysis. It has fifteen topical categories and four descriptors (see below). A review can receive several categories at once.
4. **Human annotation.** A sample of 200 reviews, stratified proportionally by platform and star rating, was coded independently by the two annotators. Agreement per category (Cohen's kappa) ranged from 0.58 to 0.91, with a mean of 0.74. Disagreements were then resolved to create a reconciled gold standard.
5. **LLM annotation.** A large language model (DeepSeek V4 Flash, high reasoning effort) coded the same 200 reviews. Against the gold standard it reached macro-F1 0.84 and mean kappa 0.71, close to the agreement between the two human annotators. The same configuration was then used to code the full corpus.

## Files

| File | Reviews | Annotation |
|---|---|---|
| `minecraft_education_reviews.csv` | 26,836 | LLM |
| `minecraft_education_validation_set.csv` | 200 | Human (reconciled gold standard) |

The two files do not overlap: the 200 human-annotated reviews are excluded from the main file. Together they form the full corpus of 27,036 reviews. Both files have the same columns.

Reviewer names and storefront countries have been removed.

## Fields

| Column | Description |
|---|---|
| `review_id` | Review identifier assigned by the store |
| `platform` | `google_play` or `apple_store` |
| `rating` | Star rating, 1–5 |
| `title` | Review title (Apple App Store only; empty for Google Play) |
| `text` | Review body, unedited (spelling, punctuation and emoji preserved; line breaks replaced with spaces) |
| `date` | Review timestamp, ISO 8601, UTC |
| `app_version` | App version at the time of the review, where available |
| `thumbs_up` | Number of "helpful" votes (Google Play only; empty for Apple App Store) |
| `primary_code` | The category that best describes the review |
| `secondary_codes` | Other applicable categories, separated by `\|` (may be empty) |
| `sentiment` | `positive`, `negative`, `mixed`, `neutral/unclear` |
| `specificity` | `low_information`, `general_opinion`, `specific_experience`, `feature_request`, `narrative` |
| `speaker_role` | `student/player`, `parent/caregiver`, `teacher/educator`, `unclear` |
| `store_description_alignment` | Whether the review supports or contradicts the official store description: `agreement`, `disagreement`, `mixed`, `not_applicable_unclear` |

A review's full set of categories is `primary_code` plus `secondary_codes`.

## Categories

| Category | Covers |
|---|---|
| `ACCESS_ACCOUNT_LICENSE` | Sign-in, Microsoft or school accounts, licensing, credential and eligibility barriers |
| `CLASSROOM_LEARNING_CONTEXT` | Use as a learning or classroom tool, educational value, or claims that the app is not educational |
| `FREE_MINECRAFT_COMPARISON` | Comparisons with regular or paid Minecraft (Bedrock, Java, Pocket Edition), or use as a free substitute |
| `LOW_INFORMATION_AFFECT` | Very short reactions with little interpretable reason ("love it", "trash") |
| `TECH_BUG_PERFORMANCE` | Crashes, lag, glitches, loading and install failures, save loss, breakage after updates |
| `MULTIPLAYER_SOCIAL_CONNECTION` | Joining friends, classmates, servers and worlds; cross-school restrictions; connection failures |
| `UPDATE_VERSION_PARITY` | Delayed updates, old versions, parity with mainstream Minecraft releases |
| `FEATURE_CONTENT_REQUEST` | Requests to add, remove or change content and features |
| `WORLDS_BUILDING_PROJECTS` | Building, world creation and ownership, saving or losing worlds, creative or survival play |
| `DEVICE_CONTROLS_PLATFORM` | Device- and platform-specific experience: Chromebook, iPad, phone, controller, touch, UI |
| `PARENT_TEACHER_MEDIATED_VOICE` | Reviews written by a parent, caregiver or educator rather than only a player |
| `TRUST_SAFETY_ACCOUNT_ANXIETY` | Fear, distrust, scam, hack or virus claims, privacy anxiety |
| `HUMOR_MEME_OFFTOPIC` | Meme language, sarcasm, performative or off-topic content |
| `GENERAL_POSITIVE_EXPERIENCE` | Substantive positive reviews without a more specific topic |
| `GENERAL_NEGATIVE_OR_REJECTION` | Rejection or dislike without a specific, actionable complaint |

## Notes

- Labels in the main file are model outputs, validated against human coding on the 200-review sample. They are not direct statements by users.
- Store reviews are self-selected: people tend to write after very good or very bad experiences, so the corpus over-represents both ends of the rating scale. Most reviewers are individual downloaders rather than teachers or schools.
- The corpus is limited to English and to the two mobile stores. It is not a complete record of every review ever submitted.
