# Quickstart: Thiết lập cơ sở dữ liệu cho Finance Agent

## Mục tiêu
Tài liệu này hướng dẫn nhanh cách thiết lập và sử dụng cơ sở dữ liệu cho hệ thống Finance Agent sau khi thiết kế lược đồ.

## Yêu cầu hệ thống
- Node.js v18+ (theo TECH.md)
- MySQL 8.0+
- pnpm (theo TECH.md)
- Docker và Docker Compose (theo milestone-01)

## Cài đặt nhanh

### 1. Cài đặt các phụ thuộc
```bash
cd finance-agent-app/backend
pnpm install
```

### 2. Cấu hình cơ sở dữ liệu
Tạo hoặc cập nhật file `.env` trong thư mục `finance-agent-app/backend`:

```env
# Database Configuration
DB_HOST=localhost
DB_PORT=3306
DB_NAME=finance_agent_db
DB_USER=finance_user
DB_PASSWORD=secure_password
DB_SYNC=false  # Nên để false trong production, chỉ dùng true trong development
DB_LOGGING=true

# JWT Configuration
JWT_SECRET=your_jwt_secret_here
JWT_EXPIRES_IN=24h

# MySQL Connection Pool
DB_CONNECTION_LIMIT=10
```

### 3. Khởi tạo cơ sở dữ liệu
Chạy lệnh sau để khởi tạo cơ sở dữ liệu:

```bash
# Tạo database nếu chưa tồn tại
mysql -u root -p -e "CREATE DATABASE IF NOT EXISTS finance_agent_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;"

# Tạo user và cấp quyền
mysql -u root -p -e "CREATE USER IF NOT EXISTS 'finance_user'@'localhost' IDENTIFIED BY 'secure_password'; GRANT ALL PRIVILEGES ON finance_agent_db.* TO 'finance_user'@'localhost'; FLUSH PRIVILEGES;"
```

### 4. Cài đặt TypeORM và các phụ thuộc liên quan
```bash
cd finance-agent-app/backend
pnpm add typeorm mysql2 reflect-metadata
pnpm add -D @types/node
```

### 5. Cấu hình TypeORM trong NestJS
Cập nhật file `src/config/database.config.ts`:

```typescript
import { ConfigModule, ConfigService } from '@nestjs/config';
import { TypeOrmModuleAsyncOptions } from '@nestjs/typeorm';

export const typeOrmConfig: TypeOrmModuleAsyncOptions = {
  imports: [ConfigModule],
  inject: [ConfigService],
  useFactory: (configService: ConfigService) => ({
    type: 'mysql',
    host: configService.get<string>('DB_HOST'),
    port: configService.get<number>('DB_PORT'),
    username: configService.get<string>('DB_USER'),
    password: configService.get<string>('DB_PASSWORD'),
    database: configService.get<string>('DB_NAME'),
    entities: [__dirname + '/../models/**/*.entity{.ts,.js}'],
    synchronize: configService.get<boolean>('DB_SYNC'), // Chỉ dùng trong development
    logging: configService.get<boolean>('DB_LOGGING'),
    retryAttempts: 3,
    retryDelay: 2000,
    autoLoadEntities: true,
  }),
};
```

### 6. Cấu hình Database Module
Tạo file `src/database/database.module.ts`:

```typescript
import { Module } from '@nestjs/common';
import { TypeOrmModule } from '@nestjs/typeorm';
import { typeOrmConfig } from '../config/database.config';

@Module({
  imports: [TypeOrmModule.forRootAsync(typeOrmConfig)],
})
export class DatabaseModule {}
```

### 7. Tạo các Entity Models
Tạo các entity trong thư mục `src/models/` theo thiết kế trong data-model.md:

Ví dụ cho `User` entity (`src/models/user.entity.ts`):

```typescript
import {
  Entity,
  PrimaryGeneratedColumn,
  Column,
  CreateDateColumn,
  UpdateDateColumn,
  OneToMany,
} from 'typeorm';
import { PortfolioItem } from './portfolio-item.entity';
import { ChatSession } from './chat-session.entity';
import { AuthToken } from './auth-token.entity';

@Entity('users')
export class User {
  @PrimaryGeneratedColumn('uuid')
  id: string;

  @Column({ unique: true, nullable: false })
  email: string;

  @Column({ nullable: false })
  passwordHash: string;

  @Column({ nullable: true })
  firstName?: string;

  @Column({ nullable: true })
  lastName?: string;

  @Column({ nullable: false, default: true })
  isActive: boolean;

  @CreateDateColumn()
  createdAt: Date;

  @UpdateDateColumn()
  updatedAt: Date;

  @OneToMany(() => PortfolioItem, (portfolioItem) => portfolioItem.user)
  portfolioItems: PortfolioItem[];

  @OneToMany(() => ChatSession, (chatSession) => chatSession.user)
  chatSessions: ChatSession[];

  @OneToMany(() => AuthToken, (authToken) => authToken.user)
  authTokens: AuthToken[];
}
```

### 8. Tạo và chạy migrations
```bash
# Tạo migration mới
pnpm typeorm migration:create src/migrations/CreateInitialSchema

# Chạy migration
pnpm typeorm migration:run -d dist/database/data-source.js
```

## Cách sử dụng cơ sở dữ liệu trong service

### Repository Pattern
```typescript
import { Injectable } from '@nestjs/common';
import { InjectRepository } from '@nestjs/typeorm';
import { Repository } from 'typeorm';
import { User } from '../models/user.entity';

@Injectable()
export class UserService {
  constructor(
    @InjectRepository(User)
    private userRepository: Repository<User>,
  ) {}

  async findAll(): Promise<User[]> {
    return await this.userRepository.find();
  }

  async findOne(id: string): Promise<User> {
    return await this.userRepository.findOneBy({ id });
  }

  async create(user: User): Promise<User> {
    return await this.userRepository.save(user);
  }
}
```

### Đăng ký Repository trong Module
```typescript
import { Module } from '@nestjs/common';
import { TypeOrmModule } from '@nestjs/typeorm';
import { UserService } from './user.service';
import { User } from '../models/user.entity';

@Module({
  imports: [TypeOrmModule.forFeature([User])],
  providers: [UserService],
  exports: [UserService],
})
export class UserModule {}
```

## Docker Compose Setup
Cập nhật file `docker/docker-compose.yml` để bao gồm MySQL:

```yaml
version: '3.8'

services:
  mysql:
    image: mysql:8.0
    restart: always
    environment:
      MYSQL_DATABASE: ${DB_NAME:-finance_agent_db}
      MYSQL_USER: ${DB_USER:-finance_user}
      MYSQL_PASSWORD: ${DB_PASSWORD:-secure_password}
      MYSQL_ROOT_PASSWORD: root_password
    ports:
      - "${DB_PORT:-3306}:3306"
    volumes:
      - mysql_data:/var/lib/mysql
      - ./init:/docker-entrypoint-initdb.d

  backend:
    build:
      context: ../finance-agent-app/backend
      dockerfile: Dockerfile
    depends_on:
      - mysql
    environment:
      DB_HOST: mysql
      DB_PORT: 3306
      DB_NAME: ${DB_NAME:-finance_agent_db}
      DB_USER: ${DB_USER:-finance_user}
      DB_PASSWORD: ${DB_PASSWORD:-secure_password}
    ports:
      - "3000:3000"

volumes:
  mysql_data:
```

## Kiểm tra kết nối
Sau khi thiết lập, bạn có thể kiểm tra kết nối cơ sở dữ liệu bằng cách:

1. Chạy ứng dụng backend:
```bash
cd finance-agent-app/backend
pnpm run start:dev
```

2. Kiểm tra log để đảm bảo kết nối cơ sở dữ liệu thành công

## Gợi ý phát triển
1. Luôn sử dụng migrations thay vì `synchronize: true` trong môi trường production
2. Thực hiện backup định kỳ cho cơ sở dữ liệu
3. Sử dụng connection pooling để tối ưu hiệu suất
4. Áp dụng phân trang cho các truy vấn trả về nhiều dữ liệu (FR-014)
5. Đảm bảo rằng người dùng chỉ truy cập dữ liệu thuộc về họ (FR-002)

## Xử lý sự cố
- Nếu gặp lỗi kết nối, kiểm tra lại cấu hình trong `.env` và đảm bảo MySQL đang chạy
- Nếu gặp lỗi với UUID, đảm bảo rằng MySQL hỗ trợ UUID (MySQL 8.0+)
- Nếu gặp lỗi với JSON fields, đảm bảo phiên bản MySQL hỗ trợ kiểu dữ liệu JSON