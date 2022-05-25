# Mini project CN262 GTP Tower Parking circuit
# GTP Tower

GTP Tower 

อาคารจอดรถแบบtower 2แถว 5ชั้น

# Description Circuit in project
# Count Car

ค่าinput จะมี parking และexit

จะทำการนับจำนวนรถ เมื่อ parking -1 และ เมื่อ exit +1

การทำงานจะมีเงื่อนไขเมื่อที่จอดว่างจะไม่สามารถให้ข้อมูลของexit ผ่าน

และเมื่อที่จอดเต็มจะไม่อนุญาติให้ข้อมูลparking ผ่าน

- counter - maxจะอยู่ที่10 และจะทำการนับจำนวนรถ เมื่อ parking จะลด-1 และ เมื่อ exit จะเพิ่ม1 ให้กับค่าของตัว count ของ counter
- display
    - d1คือแสดงผลในหลักหน่วย
    - d2คือแสดงผลในหลักสิบ

ค่าoutput ออกมาจะเป็น 8bit ของทั้ง d1และd2 เพื่อต่อเข้า 7 segment

- all available ที่จอดรถว่างทั้งหมด
- full parking ที่จอดรถเต็ม

# Process input

จะมีการใช้mux , D flip-flop และ clock

จะมีค่าinput ปุ่มตัวเลขที่กด , ปุ่มparking ,ปุ่มexit

และinput ที่ได้รับมากจากวงจรCount_car

 full parking และ all available

- Mux ตัวแรกจะมีการต่อเข้าenabled ไว้เพื่ออนุญาติข้อมูลผ่านได้ต่อเมื่อมีการกดปุ่มexit หรือ parking
- Mux ตัวที่สองจะเป็นการให้ค่าในแต่ละตัวเลขแบบ5bit อย่างเช่นเมื่อกด1 ค่าที่ออกมาจะเป็นค่าเป็น 00001
- D flip flop นำมาเพื่อใช้กับ button เนื่องจากค่าที่เข้ามาเป็นในลักษณะ short term
    
    การต่อเข้าที่ Asynchronous set เมื่อค่าเป็น 1 flipflop value ก็จะpin ที่1โดยไม่สนใจclock ทำให้ได้ค่า output ออกมาเป็น 1
    
- full parking คือ เมื่อที่จอดรถเต็มแล้วจะไม่อนุญาติให้ส่งข้อมูลของ parking
- all available คือ  เมื่อที่จอดรถว่างจะไม่อนุญาติให้ส่งข้อมูลของ exit
- Muxในส่วนของfull parkingและ all available ก็จะการต่อ เข้า enabled เพื่อกำหนดการอนุญาติให้ข้อมูลผ่านได้หรือไม่
- register ที่จะเก็บค่าเพื่ออนุญาติให้ข้อมูลของตัวเลขผ่านได้ในการกดตัวเลขทั้งสองครั้ง

ค่าoutput จะมีปุ่มที่กด , ค่าเลข5bit ของตัวเลขนั้น ,parking และ exit

# Output and Display

รับค่าinputจาก วงจรProcess input 

- ในส่วนของmuxส่วนแรกจะทำหน้าที่เป็นตัวที่ให้ค่าข้อมูลแบบ 5bit เมื่อมีการกดปุ่มตัวเลขนั้น เช่นเมื่อกด1 ค่าที่ออกมาคือ 00001
- ในวงจร or ของการกดปุ่ม ผลออกมาคือ count_button ซึ่งไว้นับว่าเรากดปุ่มในส่วนของnumpad ไปกี่ครั้ง
- register first index  คือการเก็บค่าของตัวเลขที่กดไปตัวแรกและจะแสดงผลออกมาที่d2 เมื่อcount_mux count เท่ากับ 2
- display
    - d1คือแสดงผลในหลักหน่วย
    - d2คือแสดงผลในหลักสิบ
    - ทั้งสองตัวจะให้output ออกมาเป็น 8bit เพื่อนำไปต่อกับ 7 segment

ค่าoutput จะมีส่วนของ5bit ที่ถูกแยกแแกมาทีละbit เพื่อทำในลักษณะของ button

# Calculate_input

รับค่า5bit จากวงจร output_display , reset จะreset เมื่อมีการกด ปุ่ม exit หรือ parking

การทำงานจะแบ่งเป็นสองช่วงคือ

- clockแรก จะเก็บข้อมูล 5bit ของการกดปุ่มตัวเลข ครั้งแรกโดยเก็บผ่าน Register first index และนำข้อมูลตัวแรกไปคูณกับสิบ เพื่อให้เป็นหลักสิบ
- clockสอง จะนำข้อมูลที่เก็บใน register ไปบวกกับข้อมูลที่ผ่านมาในclockที่สอง
- mux จะอนุญาติให้ข้อมูลผ่านเมื่อ counter มีค่าเท่ากับ2 คือมีการกดปุ่มตัวเลข 2 ครั้ง

output จะมี complete_num ,counter, parking ,exit

# Input_available

input 5bit จาก calculate_input

ไว้แยกว่าค่า5bit ที่รับมามีค่าตรงกับเลขไหนและส่งoutputออกไป

# available

รับค่าว่าเป็นpositionไหนจาก input_available

parking,exit,counter รับค่ามาจาก caculate_num

และ reset รับค่ามาจาก busy

- registerตัวแรกจะทำการเก็บค่า เมื่อมีข้อมูลผ่านมา
- mux  เมื่อเป็น1 จะเป็น parking  และเมื่อเป็น0 คือเป็น exit จะให้ข้อมูลผ่านก็ต่อเมื่อ เป็นpositionที่ได้รับมาจากวงจรinput_available
- register ตัวที่สอง ไว้เก็บค่าของข้อมูลจะทำงานเมื่อcounter มีค่าเท่ากับ 2

output ออกมาเป็นค่าของpositionต่างๆ

# Where to go

รับ inputจากcalculate_num เป็น5bit เพื่อนำมาเทียบว่าต้องไปชั้นไหนโดยดูจากหลักหน่วย

# G

รับinput จาก วงจรwhere to go 

G นั้นเป็นวงจรที่ประกอบด้วย D flip-flop และจำนวนของD flip-flopสอดคล้องกับจำนวนชั้นที่จะต้องการไป

เช่น G5 คือจะไปชั้น5 โดยต้องมีD flip flop 5ตัวเพื่อเก็บค่านี้เป็นเวลา5clock

ที่ต้องออกแบบเป็นแบบนี้เพื่อที่จะให้liftขึ้นทีละชั้น

# Main_logic

รับค่า state และ ค่าว่าต้องการไปชั้นไหนจาก วงจรGต่างๆ

โดยเริ่มต้นจะอยู่ที่ชั้น f ตลอดเมื่อไม่ได้รับค่าใดๆเข้ามา

จะไปชั้นอื่นๆจากinputที่ได้รับ เมื่อถึงชั้นที่inputป้อนเข้ามาก็จะกลับไปชั้นf โดยการลงจะเป็นแบบทีละชั้น

# Reference
https://youtu.be/6AoEob5dM-s
คลิปวิดีโอสาธิตการทำงานเเล้วอธิบายวงจรพื้นฐานภายในโปรเจค
