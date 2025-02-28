## V0 link to visualize the schedule

https://v0-json-to-calendar-view-9q.vercel.app/

## Prompt used that include details

"Create a TypeScript program in that generates a school timetable based on activities and constraints, loading input from JSON files and saving the output to a JSON file. The program must meet the following requirements:

Entities and Input
Define the following TypeScript interfaces, each loaded from a separate JSON file under a 'data' folder:

interface ClassType { id: string; subjects: { subjectName: string; activities: Activity[] }[]; }
interface Activity { duration: number; subject: string; sessionType: string; } (e.g., 'lecture', 'lab')
interface Class { id: string; classTypeId: string; teachers: Record<string, string>; preferredClassroom?: string; } (teachers map subject names to teacher IDs)
interface Teacher { id: string; availability: Record<string, string[]>; } (e.g., {'Monday': ['09:00-12:00']})
interface Classroom { id: string; allowedSessionTypes: string[]; } (e.g., ['lab', 'lecture'])
interface Subject { id: string; preferredStartTimes: string[]; } (e.g., ['08:00', '09:00']; empty means any time)
interface SchoolSchedule { studyDays: string[]; studyHours: { start: string; end: string; }; breaks: { start: string; end: string; }[]; timeSlot: number; maxGaps: number; }
Input files: classTypes.json, classes.json, teachers.json, classrooms.json, subjects.json, schoolSchedule.json. Include a function to load and validate JSON data from these files, ensuring type safety and required fields.

Constraints
Must-Respect Constraints (Mandatory, No Exceptions):
All sessions must be scheduled without skipping any.
Sessions must occur within studyDays and studyHours, excluding breaks, from schoolSchedule.json.
Sessions must start at intervals defined by timeSlot (e.g., 60 minutes → 08:00, 09:00, not 08:14).
No two sessions for the same Class, Teacher, or Classroom can overlap in time.
Sessions cannot be split (e.g., a 120-minute session must be contiguous).
sessionType must match one of the allowedSessionTypes of the assigned Classroom.
A Class cannot have the same subject scheduled twice on the same day.
Teacher availability (e.g., 'Monday: 09:00-12:00') must be respected for all assigned sessions.

Should-Respect Constraints (Optimize Where Possible):
Respect preferredStartTimes from Subject, snapping to the nearest valid timeSlot if possible.
Prioritize a Class’s preferredClassroom if available.
Minimize gaps between sessions for Class and Teacher, keeping gaps ≤ maxGaps from schoolSchedule.json.

Pre-Generation Validation:
Before generating the timetable, check for impossible conditions and halt with clear user feedback if detected:
Overbooked Teacher: Total session hours for a Teacher exceed their availability across all days.
Overbooked Class: Total session hours for a Class exceed available slots within studyHours and studyDays.
Insufficient Classrooms: Not enough Classrooms with required allowedSessionTypes for all sessions (e.g., too many 'lab' sessions). Provide specific feedback, e.g., 'Teacher T1 is overbooked by 4 hours on Monday; adjust availability in teachers.json.' Add new impossibility checks as discovered during development, documenting reasons clearly.

Algorithm and Progress:
Do not declare a schedule impossible unless all possible combinations have been exhausted. Use a robust algorithm like backtracking or a genetic algorithm to explore solutions:
Backtracking: Systematically try placing each session, backtracking when constraints fail, until a valid schedule is found or all options are exhausted.
Genetic Algorithm: Evolve a population of potential schedules, optimizing for constraints over iterations. Print progress updates (e.g., 'Processed 25% of sessions', 'Iteration 50/1000') to the console, as these algorithms may take time, ensuring the user knows the process is ongoing.

Validation Scripts:
Under a 'validation' folder, create scripts to run automatically after each timetable generation and after any code change:
Verify all must-respect constraints (e.g., no overlaps, correct timeSlot alignment, Teacher availability respected).
If any fail, analyze the cause (e.g., 'Class 1A has overlapping sessions at 09:00 on Monday'), log it, and iteratively fix the generation logic until all pass.

Conflict Handling:
If a perfect schedule is impossible after pre-checks pass and the algorithm completes, identify why (e.g., insufficient slots due to constraints), log the reason, and suggest adjustments to the user. Update pre-generation checks with new impossibility cases as identified.

Implementation:
Use generic logic: no hard-coded names (e.g., 'Teacher John') or fixed counts; handle any number of Class, Teacher, etc., dynamically via maps and arrays.

Output:
Generate a timetable as an array of Session objects:
interface Session { classId: string; teacherId: string; subject: string; classroomId: string; day: string; startTime: string; endTime: string; } Save to timetable.json in the project root.

Focus:
Ensure must-respect constraints are always met, validated post-generation via scripts; fix any failures iteratively.
Check for impossible cases before generation, pausing with clear feedback until resolved.
Keep the program generic and reusable across different input datasets."

PS: don't create new mock data, use the provided one and also don't forget to test your work at the end using the validation scripts mentioned above.
