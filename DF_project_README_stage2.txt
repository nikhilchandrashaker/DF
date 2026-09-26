DF — Designated Fielder Project
Stage 2: Individual suitability, team-level simulation, paper, deck
================================================================

This stage builds on df_player_team_season.csv (stage 1) and adds the
actual DF valuation layer, kept in two deliberately separate pieces:

1. An INDIVIDUAL suitability index (who looks like a DF)
2. A TEAM-LEVEL swap simulation (how much would a specific team gain)

The individual index is an input to the team simulation's reporting, not
something that auto-generates the team-level answer — see the paper,
Section 3.4, for why that separation matters (it's the paper's main
finding).

Files in this delivery
-----------------------
1. DF_suggestions_top5_by_team_season_v2.csv
   Top 5 individual DF-suitability candidates per team-season.
   Key column: DF_Suitability_Z = Defensive_Index (z-score) minus
   standardized Batting_Runs_Above_Avg. Replaces the earlier
   df_suggestion_index, which only measured defensive workload and
   said nothing about whether the player's bat made him a DF candidate.

2. DF_team_simulation_all_positions.csv
   Every team-season x position with a viable swap (an in-house
   teammate who fields the position better than the incumbent, AND an
   in-house bench bat to inherit the incumbent's plate appearances).
   Reports Net_Offensive_Swap_Runs (real batting runs, no defensive
   assumption), Defensive_Index_Gap (unitless, directional only), and
   Breakeven_Defensive_Runs_Required (how much defensive edge the DF
   candidate would need to provide, above the incumbent, to make an
   offense-negative swap worthwhile overall).

3. DF_team_simulation_best_swap_by_season.csv
   One row per team-season: the single highest Net_Offensive_Swap_Runs
   position from file #2, joined with team win/loss/run context.

4. DF_research_paper.docx
   Full write-up: methods, the Range-Factor-to-runs conversion pitfall
   (and why it was abandoned), the team-simulation design, results
   tables, case studies (1985 Cardinals, 2000 Cardinals), limitations.

5. df_deck.html (published as a Claude artifact)
   Seven-slide walkthrough of the same material for a live presentation.

Important, still true from stage 1
-----------------------------------
- All batting and fielding inputs are observed Lahman statistics.
- No WAR is fabricated. Batting_Runs_Above_Avg is Pete Palmer's
  published fixed linear-weights formula, era-normalized by subtracting
  each league-year's own average of the same formula (not a different
  constant, and not an invented external number).
- No defensive-runs metric is fabricated. Defensive_Index is a
  position-season z-score (unitless) precisely because Lahman's
  Fielding table cannot support a valid runs conversion on its own —
  see the paper, Section 3.2, for the diagnostic that surfaced this.
- The team-level Net_Offensive_Swap_Runs is the only figure in this
  delivery denominated in real runs on the defense/offense trade-off
  question; everything else is either an observed statistic or an
  explicitly unitless standardized index.

Known limitations to keep in mind before citing a specific case
-----------------------------------------------------------------
- The "bench-level PA" window (80-350 approximate PA) used to find a
  displaced hitter can misfire on a player who was traded or injured
  mid-season rather than one genuinely buried for a defensive weakness
  (see the Mark McGwire / 2000 Cardinals cases in the paper).
- Pre-1954-ish seasons often record a generic "OF" rather than
  LF/CF/RF, inflating the outfield candidate pool relative to infield
  positions in the position-frequency breakdown.
- Eligibility requires >=150 innings at a position (for a defensive
  comparison) and 80-350 approximate PA (for the bench-bat search);
  team-seasons without both fail to produce a swap and are simply
  absent from files #2 and #3, most heavily in the 19th century when
  rosters were too small to carry a genuine bench.

Source
------
SABR / Sean Lahman Baseball Database, 1871-2025, released December 10, 2025.
