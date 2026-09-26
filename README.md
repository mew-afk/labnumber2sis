public class RabbitAndTurtle {

    public static void main(String[] args) throws InterruptedException {
        AnimalThread rabbit = new AnimalThread("Кролик", Thread.MAX_PRIORITY);
        AnimalThread turtle = new AnimalThread("Черепаха", Thread.MIN_PRIORITY);

        rabbit.start();
        turtle.start();

        for (int i = 0; i < 10; i++) {
            Thread.sleep(200);

            if (rabbit.getMeters() > turtle.getMeters()) {
                rabbit.setPriority(Thread.MIN_PRIORITY);
                turtle.setPriority(Thread.MAX_PRIORITY);
            } else {
                turtle.setPriority(Thread.MIN_PRIORITY);
                rabbit.setPriority(Thread.MAX_PRIORITY);
            }
        }

        rabbit.join();
        turtle.join();

        System.out.println("Забег окончен!");
        System.out.println("Кролик: " + rabbit.getMeters() + " метров");
        System.out.println("Черепаха: " + turtle.getMeters() + " метров");
        }
    }

class AnimalThread extends Thread {

    private String animalName;
    private int meters = 0;

    public AnimalThread(String name, int priority) {
        this.animalName = name;
        setPriority(priority);
    }

    public int getMeters() {
        return meters;
    }

    @Override
    public void run() {
        for (int i = 0; i < 10; i++) {
            meters += getPriority();
            System.out.println(animalName + " пробежал(а) " + meters + " метров");
            try {
                Thread.sleep(200);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
        System.out.println(animalName + " закончил(а) бег. Итог: " + meters + " метров");
    }
}
