
Two-Phase commit: pattern

It's ACID

Implementation using atomikos

```
import com.atomikos.icatch.jta.UserTransactionImp;
import com.atomikos.jdbc.AtomikosDataSourceBean;
import jakarta.transaction.UserTransaction;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.util.Properties;

public class TwoPhaseCommitExample {

    // Database 1: PostgreSQL Data Source (XA-compliant)
    private static AtomikosDataSourceBean createPostgresXADataSource() {
        AtomikosDataSourceBean ds = new AtomikosDataSourceBean();
        ds.setUniqueResourceName("postgresDB");
        ds.setXaDataSourceClassName("org.postgresql.xa.PGXADataSource");
        
        Properties props = new Properties();
        props.setProperty("user", "postgres_user");
        props.setProperty("password", "password123");
        props.setProperty("serverName", "localhost");
        props.setProperty("portNumber", "5432");
        props.setProperty("databaseName", "order_db");
        ds.setXaProperties(props);
        
        ds.setPoolSize(5);
        return ds;
    }

    // Database 2: MySQL Data Source (XA-compliant)
    private static AtomikosDataSourceBean createMysqlXADataSource() {
        AtomikosDataSourceBean ds = new AtomikosDataSourceBean();
        ds.setUniqueResourceName("mysqlDB");
        ds.setXaDataSourceClassName("com.mysql.cj.jdbc.MysqlXADataSource");
        
        Properties props = new Properties();
        props.setProperty("user", "mysql_user");
        props.setProperty("password", "password123");
        props.setProperty("serverName", "localhost");
        props.setProperty("portNumber", "3306");
        props.setProperty("databaseName", "inventory_db");
        ds.setXaProperties(props);
        
        ds.setPoolSize(5);
        return ds;
    }

    public static void main(String[] args) {
        // Initialize DataSources
        AtomikosDataSourceBean postgresDS = createPostgresXADataSource();
        AtomikosDataSourceBean mysqlDS = createMysqlXADataSource();

        // Get the JTA UserTransaction (The Coordinator)
        UserTransaction utx = new UserTransactionImp();

        try {
            // PHASE 1 START: Begin the global transaction
            utx.begin();

            // Operations on Database 1 (PostgreSQL)
            try (Connection conn1 = postgresDS.getConnection();
                 PreparedStatement ps1 = conn1.prepareStatement(
                     "INSERT INTO orders (id, item, status) VALUES (?, ?, ?)")) {
                ps1.setInt(1, 101);
                ps1.setString(2, "Laptop");
                ps1.setString(3, "PENDING");
                ps1.executeUpdate();
            }

            // Operations on Database 2 (MySQL)
            try (Connection conn2 = mysqlDS.getConnection();
                 PreparedStatement ps2 = conn2.prepareStatement(
                     "UPDATE inventory SET stock = stock - 1 WHERE item_name = ?")) {
                ps2.setString(1, "Laptop");
                ps2.executeUpdate();
            }

            // PHASE 2: Commit. Atomikos handles the "Prepare" votes internally.
            // If both DBs vote YES, it commits. If either fails, it throws an exception.
            utx.commit();
            System.out.println("Transaction successfully committed across both databases via 2PC.");

        } catch (Exception e) {
            System.err.println("Transaction failed! Rolling back changes on all databases. Reason: " + e.getMessage());
            try {
                // If anything goes wrong, rollback both databases
                utx.rollback();
            } catch (Exception rollbackEx) {
                rollbackEx.printStackTrace();
            }
        } finally {
            // Clean up resources
            postgresDS.close();
            mysqlDS.close();
        }
    }
}

```