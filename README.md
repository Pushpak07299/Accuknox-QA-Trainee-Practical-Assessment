import java.io.File;
import java.lang.management.ManagementFactory;
import com.sun.management.OperatingSystemMXBean;
import java.text.SimpleDateFormat;
import java.util.Date;

public class SystemHealthMonitor {
    private static final double CPU_THRESHOLD = 80.0;
    private static final double MEMORY_THRESHOLD = 80.0;
    private static final double DISK_THRESHOLD = 80.0;

    public static void main(String[] args) {
        monitorSystemHealth();
    }

    private static void monitorSystemHealth() {
        OperatingSystemMXBean osBean = ManagementFactory.getPlatformMXBean(OperatingSystemMXBean.class);
        
        double cpuLoad = osBean.getSystemCpuLoad() * 100;
        double totalMemory = osBean.getTotalPhysicalMemorySize();
        double freeMemory = osBean.getFreePhysicalMemorySize();
        double memoryUsage = ((totalMemory - freeMemory) / totalMemory) * 100;
        
        File diskPartition = new File("/");
        double totalDiskSpace = diskPartition.getTotalSpace();
        double freeDiskSpace = diskPartition.getFreeSpace();
        double diskUsage = ((totalDiskSpace - freeDiskSpace) / totalDiskSpace) * 100;

        String timestamp = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss").format(new Date());
        
        if (cpuLoad > CPU_THRESHOLD) {
            System.out.println(timestamp + " - ALERT: CPU usage exceeds threshold: " + cpuLoad + "%");
        }
        
        if (memoryUsage > MEMORY_THRESHOLD) {
            System.out.println(timestamp + " - ALERT: Memory usage exceeds threshold: " + memoryUsage + "%");
        }

        if (diskUsage > DISK_THRESHOLD) {
            System.out.println(timestamp + " - ALERT: Disk usage exceeds threshold: " + diskUsage + "%");
        }
    }
}
