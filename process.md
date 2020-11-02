Tiến trình: hình ảnh trừu tượng của 1 chương trình đang chạy. Cung cấp khả năng (giả) thực thi đồng thời các chương trình khi chỉ có 1 CPU, chuyển 1 CPU thành nhiều CPU ảo.

## Tiến trình

- Program counter, registers, variables.

- Mỗi tiến trình có 1 bộ đếm chương trình(counter) logic riêng. Mỗi khi 1 tiến trình chạy thì bộ đếm này được nạp vào bộ đếm thực. Khi chuyển tiến trình, giá trị bộ đếm thực được lưu vào bộ đếm logic của process trên bộ nhớ. 

- a process is an activity of some kind. It has a program, input, output, and a state

## Tạo tiến trình

4 sự kiện tạo ra 1 tiến trình mới:

- Khởi tạo hệ thống

- lời gọi hệ thống tạo tiến trình mới được gọi bởi 1 tiến trình đang chạy

- 1 người dùng yêu cầu tạo 1 process mới

- Khởi tạo batch

Khi hệ điều hành được boot thì rất nhiều tiến trình chạy ngầm hoặc chạy "nổi" đều được tạo. Ngoài các tiến trình được tạo khi khởi động, thì nhiều tiến trình mới cũng đc tạo sau đó. 1 tiến trình đang chạy sử dụng lời gọi hệ thông để tạo 1 hay nhiều tiến trình mới để giúp nó thực hiện công việc. Trong các hệ tương tác, người dùng có thể chạy chương trình bằng cách gõ 1 câu lệnh hoặc nháy chuột vào biểu tượng. Việc này tạo ra 1tiến trình mới và chạy chương trình đã chọn trong nó. Trong các hệ máy tính lớn, người dùng có thể gửi 1 loạt các công việc cho hệ thống, hệ điều hành sẽ chọn từng việc và tạo ra 1 tiến trình mới để thực hiện công việc.

Tất cả các trường hợp trên 1 tiến trình mới đều được tạo từ việc 1 tiến trình đang tồn tại thực hiện lời gọi hệ thống tạo tiến trình mới. Cả tiến trình cha và con đều có không gian bộ nhớ riêng, không ghi lên nhau được. Trong trường hợp muốn chia sẻ bộ nhớ cần thực hiện theo chiến lược copy-on-write, copy sang và sửa đổi chứ không sử trực tiếp.

## Kết thúc tiến trình 

Không có gì là mãi mãi cả. 1 tiến trình sẽ kết thúc trong 1 trong các trường hợp:

- Bình thường(tình nguyện)

- Thoát khi có lỗi(tình nguyện)

- Lỗi nghiêm trọng(không tự nguyện)

- Triệt tiêu bơi tiến trinhf khác(k tự nguyện)

Hầu hết các tiến trình kết thúc khi hoàn thành công việc. Trình dịch sẽ chỉ ra khi nào kết thúc chương trình được đưa cho tiến trìn bằng việc gọi lời gọi hệ thống ```exit```.

Trường hợp 2 xảy ra khi tiến trình phát hiện lỗi và thoát.

Trường hợp 3 xảy ra khi tiến trình tạo ra lỗi, câu lệnh không hợp lệ, tham chiếu không tồn tại, chia cho 0,...

Trường hợp 4 xảy ra khi 1 tiến trình khác thực hiện lời gọi nói với hđh hủy các tiến trình khác.

## Trạng thái tiến trình

- Running (actually using the CPU at that instant).

- Ready (runnable; temporarily stopped to let another process run).

- Blocked (unable to run until some external event happens).

Bên trong hệ thống là rất nhiều tiến trình, mỗi tiến trình chịu trách nhiệm 1 công việc. Khi có việc chúng sẽ hoạt động và được cấp CPU còn không có việc sẽ bị block.

Scheduler là thành phần thấp nhất của hệ điều hành với rất nhiều tiến trình đặt trên nó. Chịu trách nhiệm xử lí ngắt, khởi động hay dừng 1 tiến trình

## Implementation

Hệ điều hành duy trì 1 bảng gọi là <b>process table</b> với 1 process 1 mục, chứa các thông tin quan trọng về tiến trình để có thể chuyển qua lại giữa run-ready-block.

Việc chuyển qua lại giữa các tiến trình thực hiện bằng việc lấy ra và lưu lại các mục tương ứng với tiến trình trong bảng. Việc chọn tiến trình nào để chạy do bộ lập lịch thực hiện.

1 tiến trình có thể bị ngắt hàng nghìn lần trong suốt quá trình thực thi nhưng điểm quan trọng là sau mỗi lần ngắt trạng thái của nó đều được lưu lại.

# Luồng

## Usage

Mô hình|Thuộc tính
---|---
Nhiều tiến trình|Parallelism, blocking system calls
Single-threaded process|No parallelism, blocking system calls
Finite-state machine|Parallelism, nonblocking system calls, interrupts 

## Mô hình luồng 

Tiến trình dựa trên 2 ý tưởng độc lập: nhóm tài nguyên và sự thực thi.

Tiến trình là 1 cách nhóm các tài nguyên lại để dễ quản lí, mỗi tiến trình có 1 luồng thực thi-thread. 

Tiến trình được sử dụng để nhhóm tài nguyên, luồng là các thực thể được lập lịch để chạy trên CPU.

CPU chuyển qua lại giữa các tiến trình làm cho các tiến trình chạy có vẻ song song.

Nhiều luồng cùng hoạt động để  thực hiện 1 công việc.

Các luồng chia sẻ chung tài nguyên của 1 tiến trình, có thể đọc ghi lên vùng nhớ của các luồng khác trong cùng 1 tiến trình để cungf thực hiện 1 công việc nào đó.

Luồng cũng có các trạng thái tương tự tiến trình: running, blocked, ready, terminated.

Mỗi luồng có stack riêng. Mỗi stack chứa 1 frame cho mỗi thủ tục được gọi nhưng chưa trả về kết quả.

```thread_create```,```thread_exit```,```thread_yield```

## User space thread

thực hiện lưu và chuyển luồng ngay tại tiến trình chưa nó do các thủ tục trong tiến trình thực hiện. Tốc độ rất cao. Cho phép mỗi tiến trình có
thuật toán lập lịch riêng.

Bất lợi: blocking call làm toàn bộ các luồng khác phải chờ-xử lí = jacket/wraper, page fault làm toàn bộ các luồng bị block, 1 luồng có thể chiếm trọn cpu của các luồng khác nếu nó muốn vì không có ngắt.

## Luồng tại nhân 

Thông tin về luồng được lưu tại nhân. 1 luồng muốn tạo luồng mới cần gọi lời gọi hệ thống.

Nếu 1 luồng bị block thì nhân có thể chọn 1 luồng khác cùng hoặc k cùng tiến trình để chạy. Ngược lại với user-level, việc chuyển luồng do các hàm trong tiến trình thực hiện cho đến khi nhân chuyển CPU cho tiến trình khác.

Luồng có thể được tái sử dụng.

## Phương pháp lai

Sử dụng cả 2 loại luồng trong 1 tiến trình.

## Các vấn đề khi xây dựng đa luồng

Vấn đề biến toàn cục: cấp cho mỗi luồng 1 vùng nhớ để lưu biến toàn cục riêng

Về signal: khi luồng nằm trong user-level, tín hiệu sẽ được truyền đến cho luồng nào?

Về bộ nhớ: khi stack của tiến trình bị đầy, nhân cung cấp thêm bộ nhớ nhưng nếu nhân không biết về stack của các luông trong tiến trình nó không tự cấp bộ nhớ được gây lỗi...

# Giao tiếp liên tiến trình(IPC)

- Làm thế nào để 1 tiến trình có thể truyền thông tin cho tiến trình khác.

- Đảm bảo các tiến trình không giẫm chân vào nhau: 2 tiến trình cùng thực hiện đặt 1 chỗ cho 2 khách hàng khác nhau 

- Đảm bảo thứ tự thực hiện khi có phụ thuộc: A sinh ra dữ liệu để B in thì B phải đợi cho tới khi A sinh ra được dữ liệu đầu tiên trước khi in.

Các vấn đề và giải pháp tương tự với luồng.

## Race condition

Tình huống khi 2 hoặc nhiều tiến trình cùng đọc hoặc ghi vào 1 vùng dữ liệu mà kết quả lại dựa trên ai thực hiện trước gọi là <b>race condition</b>

## Critical Regions

Phần truy cập tài nguyên dẫn đến race condition gọi là <b>critical region</b> hoặc <b>critical section</b>. 

Nếu có thể tránh 2 tiến trình cungf trong critical region cùng 1 lúc ta có thể tránh được xung đột.

## Loại trừ lẫn nhau

### Tắt ngắt

1 khi tiến trình bước vào critical region, nó sẽ tắt tất cả các ngắt đi và bật lại khi qua CR, yên tâm rằng không bị chuyển sang tiến trình khác khi đang chạy.

Nhược điểm khi cho 1 tiến trình ở user tắt ngắt, nếu nó không bật ngắt lại và chiếm hết CPU, 1 hệ thống CPU đa nhân, tắt ngắt ở 1 nhân và các nhân khác vẫn hoạt động bình thường các tiến trình khác vẫn truy cập đc tài nguyên và gây xung đột.

### Khóa

Khóa là 1 biến có giá trị 0 nếu tài nguyên đang rỗi, 1 là đã có ng sử dụng. Tuy nhiên khóa cũng gặp vấn đề y như đối với race condition nếu có 2 tiến trình cùng đọc và ghi khóa.

### Strict Alternation

- Busy wating: Liên tục kiểm tra 1 biến cho đến khi giá trị nào đó đạt được. Tốn CPU.

- Spin lock: khóa sử dụng busy waiting.

Có thể có trường hợp 1 tiến trình chạy quá chậm nên chưa kịp cập nhật biến làm tiến trình còn lại phải đợi dù nó đang không trong CR.

### Giải pháp của Peterson

```
#define FALSE 0
#define TRUE 1
#define N 2 / * number of processes * /
int turn;
int interested[N]; / * whose turn is it? * /
/ * all values initially 0 (FALSE) * /
void enter region(int process);
{
int other; / * process is 0 or 1 * /
}
/ * number of the other process * /
other = 1 − process;
/ * the opposite of process * /
interested[process] = TRUE;
/ * show that you are interested * /
tur n = process;
/ * set flag * /
while (turn == process && interested[other] == TRUE) / * null statement * / ;
void leave region(int process)
{
interested[process] = FALSE;
}
/ * process: who is leaving * /
/ * indicate departure from critical region * /
```

Sử dụng busy waiting.

## Sleep And Wakeup

Busy waiting: khi 1 tiến trình muốn đi vào CR, nó kiểm tra có được k, nếu không nó sẽ thực hiện lặp lại kiểm tra cho đến khi được.

Cách này không chỉ làm phí CPU mà còn gây ra các hậu quả phụ. Ví dụ, với 1 hệ thống có 2 tiến trình H-vói độ ưu tiên cao, L với độ uư tiên thấp, khi L đang trong CR thì H yêu cầu và phải đợi trong vòng lặp. Tuy nhiên, do H có độ ưu tiên cao nên nó liên tục được cấp CPU chỉ để thực hiện lặp còn L thì k đc cấp CPU để thực hiện nốt CR. Vậy nên H cứ phải đợi mãi còn L thì không thực hiện gì cả.

Sleep-wakeup là kiểu giao tiếp liên trinhf nguyên thủy sử dụng block thay vì lặp. ```Sleep``` là 1 lời gọi hệ thống làm cho caller bị block cho đến khi 1 tiến trình khác đánh thức nó. ```wakeup``` là lời gọi hệ thống có 1 tham số dùng để đánh thức 1 tiến trình nào đó.

### Producer-Consumer

Vấn đề xảy ra khi tín hiệu đánh thức được gửi đến tiến trình chưa ngủ sẽ bị mất.

## Semaphore

Semaphore đếm số thông điệp wakeup. 0 có nghĩa là chưa có thông điệp nào được lưu, giá trị dương nếu có 1 hoặc hơn thông được đang chờ.

2 hành động trên semaphore: ```down``` và ```up```(tổng quát của sleep và wakeup) là các hành động nguyên tố <b>atomic action</b> không thể phân chia được: hoặc là thực hiện mà khôg bị ngắt hoặc là không thực hiện gì cả. Hành động này đạt được băngf phần cứng, ngắt memory bus.

semophore khác với khóa, chính là biến đếm luôn. việc đánh thức các tiến trình đang ngủ khi đợi semaphore là ngẫu nhiên.

Semaphore có 2 công dụng: loại trừ lẫn nhau để chỉ 1 tiến trình được đọc ghi tài nguyên, đồng bộ để các biến là giống nhau tại các tiến trinhd dùng chung.

## Mutex


