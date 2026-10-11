# Queen Fallback Implementation Evidence

**Source commit:** [`e7c380f`](https://github.com/REPLACE_WITH_YOUR_REPO/commit/e7c380f)

## QG_CORNER

The `QG_CORNER` implementation searches for a defensible anchor position, prioritises suitable positions near portals, keeps the queen near the selected anchor, and permits a portal escape when an enemy approaches.

### 1. Anchor search

`corner_search()` uses breadth-first search (BFS) to find candidate positions. A candidate must have kelp within reach on at least three sides, have been seen, and not be marked as unsuitable. Candidate scoring prioritises portal-associated positions and known exits, with a distance penalty.

```cpp
void corner_search() {
    corner_center = -1;
    corner_dir = -1;
    visit_mark++;
    int queue_head = 0;
    int queue_tail = 0;
    int const start = index_of(head);
    visit_stamp[start] = visit_mark;
    step_count[start] = 0;
    bfs_queue[queue_tail++] = head;
    double best = SCORE_REJECT;
    auto const directions = Direction::get_direction_list();
    while (queue_head < queue_tail) {
        Position const position = bfs_queue[queue_head++];
        int const index = index_of(position);
        int const steps = step_count[index];
        int sides = 0;
        for (Direction const direction : directions) {
            sides += kelp_within(position, direction) ? 1 : 0;
        }
        if (sides >= 3 && seen_round[index] >= 0 &&
            std::find(corner_bad.begin(), corner_bad.end(), index) == corner_bad.end()) {
            int mouth = -1;
            int mouth_dir = -1;
            bool known = false;
            for (int const cell : corner_area(index)) {
                for (std::size_t k = 0; k < directions.size(); k++) {
                    if (edge_at(position_of(cell), directions[k]) != EDGE_PORTAL) {
                        continue;
                    }
                    bool const exit_known = portal_exit(position_of(cell), directions[k]) >= 0;
                    if (mouth < 0 || (exit_known && !known)) {
                        mouth = cell;
                        mouth_dir = static_cast<int>(k);
                        known = exit_known;
                    }
                }
            }
            int const center = mouth >= 0 ? mouth : index;
            if (std::find(corner_bad.begin(), corner_bad.end(), center) == corner_bad.end() &&
                corner_room(center) >= length + 1) {
                double const score = (mouth >= 0 ? 1000.0 : 0.0) + (known ? 500.0 : 0.0) - steps;
                if (score > best) {
                    best = score;
                    corner_center = center;
                    corner_dir = mouth_dir;
                }
            }
        }
        if (steps >= QG_CORNER_REACH) {
            continue;
        }
        for (Direction const direction : directions) {
            if (edge_at(position, direction) != EDGE_OPEN) {
                continue;
            }
            int const next = index_of(position.add_dir(direction));
            if (visit_stamp[next] == visit_mark) {
                continue;
            }
            visit_stamp[next] = visit_mark;
            step_count[next] = steps + 1;
            bfs_queue[queue_tail++] = position_of(next);
        }
    }
}
```

### 2. Anchor maintenance and threat response

`corner_update()` manages the anchor's validity, periodically searches for a new position, checks for nearby enemies, and sets a movement target when the queen needs to return to the anchor or move towards an escape position.

```cpp
void corner_update() {
    corner_target = -1;
    corner_escape = false;
    corner_hold = false;
    if (feeding_phase() || (QG_SAFEZONE && zone_mode != 0 && zone_mode != 4)) {
        corner_center = -1;
        return;
    }
    if (corner_center >= 0 && corner_room(corner_center) < length + 1) {
        corner_center = -1;
    }
    if (corner_center < 0 && round - corner_round >= QG_CORNER_EVERY) {
        corner_round = round;
        corner_search();
        if (QG_CORNER_LOG && corner_center >= 0) {
            ct.output_log("CANCHOR", corner_center % width, corner_center / width, corner_dir,
                          corner_dir >= 0 && portal_exit(position_of(corner_center), Direction::get_direction_list()[static_cast<std::size_t>(corner_dir)]) >= 0 ? 1 : 0,
                          length);
        }
    }
    if (corner_center < 0) {
        return;
    }
    corner_escape = enemy_near(head, QG_CORNER_DANGER);
    int const gap = manhattan(head, position_of(corner_center));
    corner_hold = !corner_escape && gap <= QG_CORNER_RADIUS;
    if (gap > QG_CORNER_RADIUS || (corner_escape && corner_dir >= 0 && gap > 0)) {
        corner_target = corner_center;
    }
    if (QG_CORNER_LOG && gap <= QG_CORNER_RADIUS) {
        ct.output_log("CNEAR", gap, corner_escape ? 1 : 0, length);
    }
}
```

### 3. Portal escape conditions and post-move handling

The following functions check whether the queen's move matches the designated escape direction, score the portal exit, and invalidate the current anchor after an escape.

```cpp
bool corner_gate(Direction direction) const {
    return corner_escape && corner_dir >= 0 && index_of(head) == corner_center && face_code(direction) == corner_dir;
}

double corner_portal_score(Direction direction, bool& usable) {
    int const exit = portal_exit(head, direction);
    usable = exit >= 0 && enter_time[exit] <= 1;
    if (exit >= 0 && !usable) {
        return SCORE_REJECT;
    }
    return (usable ? score_step(position_of(exit)) : 0.0) - PORTAL_LEAP_PENALTY;
}

void corner_after_move() {
    if (corner_center < 0 || act_dirs.size() != 1 || !corner_gate(act_dirs.front())) {
        return;
    }
    if (QG_CORNER_LOG) {
        ct.output_log("CESCAPE", head.x, head.y, length);
    }
    corner_bad.push_back(corner_center);
    corner_center = -1;
    corner_round = round;
}
```

## Scope and limitations

These excerpts document selected parts of the strategy implementation at the referenced commit. They are not a standalone, buildable bot, and do not include all declarations, dependencies, configuration definitions, or movement-selection logic.
