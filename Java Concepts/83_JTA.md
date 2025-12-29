# JTA (Java Transaction API)

## What is JTA?
JTA provides transaction management across multiple resources.

## UserTransaction
```java
import javax.transaction.*;

public class TransactionExample {
    @Resource
    private UserTransaction userTransaction;
    
    public void transferMoney(Account from, Account to, BigDecimal amount) {
        try {
            userTransaction.begin();
            
            from.debit(amount);
            to.credit(amount);
            
            userTransaction.commit();
        } catch (Exception e) {
            try {
                userTransaction.rollback();
            } catch (SystemException se) {
                se.printStackTrace();
            }
            throw new RuntimeException(e);
        }
    }
}
```

## Container-Managed Transactions
```java
import javax.ejb.*;
import javax.transaction.Transactional;

@Stateless
public class AccountService {
    
    @TransactionAttribute(TransactionAttributeType.REQUIRED)
    public void transfer(Account from, Account to, BigDecimal amount) {
        from.debit(amount);
        to.credit(amount);
    }
    
    @TransactionAttribute(TransactionAttributeType.REQUIRES_NEW)
    public void logTransaction(TransactionLog log) {
        // Always in new transaction
    }
    
    @TransactionAttribute(TransactionAttributeType.NOT_SUPPORTED)
    public void readOnlyOperation() {
        // No transaction
    }
}
```

## Spring @Transactional
```java
import org.springframework.transaction.annotation.*;

@Service
@Transactional
public class AccountService {
    
    @Transactional(rollbackFor = Exception.class)
    public void transfer(Account from, Account to, BigDecimal amount) {
        from.debit(amount);
        to.credit(amount);
    }
    
    @Transactional(readOnly = true)
    public Account getAccount(Long id) {
        return accountRepository.findById(id);
    }
    
    @Transactional(propagation = Propagation.REQUIRES_NEW)
    public void logTransaction(TransactionLog log) {
        transactionLogRepository.save(log);
    }
}
```

## Transaction Attributes
- **REQUIRED**: Use existing or create new
- **REQUIRES_NEW**: Always create new
- **SUPPORTS**: Use if exists, otherwise no transaction
- **NOT_SUPPORTED**: Suspend if exists
- **MANDATORY**: Must exist, throw exception if not
- **NEVER**: Must not exist, throw exception if exists

## Best Practices
1. Keep transactions short
2. Handle exceptions properly
3. Use appropriate propagation
4. Avoid long-running transactions
5. Use read-only when possible
6. Handle deadlocks
7. Monitor transaction performance
8. Use two-phase commit for multiple resources

