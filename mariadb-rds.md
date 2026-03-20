```
provider "aws" {
  region = "ap-south-1"
}

resource "aws_security_group" "rds_sg" {
  name = "rds-sg"

  ingress {
    from_port   = 3306
    to_port     = 3306
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"] # ⚠️ restrict later
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

resource "aws_db_instance" "mariadb" {
  identifier         = "mariadb-db"
  engine             = "mariadb"
  engine_version     = "10.6"
  instance_class     = "db.t3.micro"
  allocated_storage  = 20

  username = "admin"
  password = "admin123"

  publicly_accessible = true
  skip_final_snapshot = true

  vpc_security_group_ids = [aws_security_group.rds_sg.id]
}

output "rds_endpoint" {
  value = aws_db_instance.mariadb.endpoint
}
```
